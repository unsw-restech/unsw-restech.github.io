# Stable Diffusion on an HPC Environment

This documentation covers how to run Stable Diffusion on a High-Performance Computing (HPC) environment, including sections on installation, inference using job scheduling, and finetuning a model.

<figure>
    <img src="../../assets/SD_logo.png" alt="Stable Diffusion" width="800">
</figure>

---

## 1. Installation

1. **Create the Working Directory**  
   Create a directory for the Stable Diffusion setup on the scratch space (or any suitable storage on your HPC). This is where all necessary files and environments will reside:
   ```bash
   export base_dir="/srv/scratch/$USER/<your directory>"
   mkdir -p $base_dir
   cd $base_dir
   ```

2. **Load the Python Module**  
   Ensure you have Python installed on your HPC and load the appropriate module (adjust based on your HPC environment):
   ```bash
   module load python
   ```

3. **Create and Activate a Virtual Environment**  
   Create a Python virtual environment for Stable Diffusion and its dependencies:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```

4. **Install Required Dependencies**  
   Install the necessary packages, such as `torch`, `diffusers`, and other essential libraries:
   ```bash
   pip install torch diffusers transformers accelerate pillow tqdm pyyaml
   ```

---

## 2. Inference using Job Scheduling

To perform inference on an HPC, job scheduling is essential to allocate resources efficiently and run tasks on available nodes. For #PBS arguments, check out [running jobs on Katana](../using_katana/running_jobs.md)

1. **Create a PBS Script**  
   A PBS script schedules the job to run on the HPC. Here's a sample script to run inference with a Stable Diffusion model:

   ```bash
   #!/bin/bash
   #PBS -l select=1:ncpus=4:ngpus=1:mem=16GB:gpu_model=V100
   #PBS -l walltime=2:00:00
   #PBS -j oe
   #PBS -N sd-inference
   #PBS -M <your_email>@unsw.edu.au
   #PBS -m abe

   # Load necessary modules
   module load python

   # Activate your virtual environment
   source $base_dir/venv/bin/activate

   # Set Hugging Face cache to scratch
   export HF_HOME="/srv/scratch/$USER/huggingface_cache"

   # Define arguments for inference
   PROMPT="A serene landscape with mountains and a lake"
   MODEL_ID="CompVis/stable-diffusion-v1-4"
   LORA_CHECKPOINT="/srv/scratch/$USER/finetune/sd-flower-model/<your checkpoint>" # leave it blank if you do not have checkpoint

   # Run the inference Python script
   python inference.py --prompt "$PROMPT" --model_id "$MODEL_ID" --lora_checkpoint "$LORA_CHECKPOINT"
   ```

!!! note
    **Important: Set the Hugging Face Cache Directory (HF_HOME)**  
    By default, Hugging Face's models and checkpoints are saved in the home directory cache, which is often limited in space (e.g., 10GB). On HPC systems, the scratch directory usually has more space (e.g., 120GB), but it is **not backed up**. You should configure Hugging Face to store its cache in the scratch directory by setting the `HF_HOME` environment variable:
    
    ```bash
    export HF_HOME="/srv/scratch/<your_user>/huggingface_cache"
    ```
    
    This will ensure that models and related files are stored in your scratch directory where you have ample space. **Important:** After completing your work, it is recommended to move any important models or files from scratch to your home directory, as scratch space is not backed up.


2. **Inference Script (`inference.py`)**  
   The inference script loads a pre-trained Stable Diffusion model, optionally applies a LoRA checkpoint, and generates images based on the input prompt.

   ```python
   import argparse
   import torch
   from diffusers import StableDiffusionPipeline
   from PIL import Image
   import matplotlib.pyplot as plt

   def inference(prompt, model_id, lora_checkpoint=None):
       # Load the model
       pipeline = StableDiffusionPipeline.from_pretrained(model_id, torch_dtype=torch.float16)
       device = "cuda" if torch.cuda.is_available() else "cpu"
       pipeline.to(device)

       # Apply LoRA checkpoint if available
       if lora_checkpoint:
           pipeline.unet.load_attn_procs(lora_checkpoint)

       # Generate images
       num_samples = 4
       generator = torch.Generator(device).manual_seed(42)
       images = pipeline(
           prompt,
           num_inference_steps=30,
           generator=generator,
           num_images_per_prompt=num_samples,
           height=512,
           width=512,
           guidance_scale=7.5
       ).images

       # Arrange images in a grid and save
       grid_image = make_grid(images, rows=2, cols=2)
       grid_image.save(f"image_grid_{prompt.replace(' ', '_')}.png")

   def make_grid(images, rows, cols):
       w, h = images[0].size
       grid = Image.new('RGB', size=(cols * w, rows * h))
       for i, img in enumerate(images):
           grid.paste(img, box=(i % cols * w, i // cols * h))
       return grid

   if __name__ == "__main__":
       parser = argparse.ArgumentParser(description="Stable Diffusion Inference")
       parser.add_argument("--prompt", type=str, required=True)
       parser.add_argument("--model_id", type=str, required=True)
       parser.add_argument("--lora_checkpoint", type=str, help="Path to LoRA checkpoint, if any")
       args = parser.parse_args()

       inference(args.prompt, args.model_id, args.lora_checkpoint)
   ```

3. **Submit the Job**  
   Use the `qsub` command to submit the PBS job:
   ```bash
   qsub <pbs_script_name>.pbs
   ```

---

## 3. Finetuning

Finetuning Stable Diffusion with your dataset on an HPC requires adjusting the model weights to fit the specific domain of your new data.

1. **Prepare Dataset**
    Check out the [Hugging Face documentation on datasets](https://huggingface.co/docs/datasets/image_dataset#imagefolder) to find out how to create metadata and place dataset in a folder in a way that is compatible with model finetuning. 


2. **Transfer dataset to Katana**
    Depending on your OS, you can use either `rsync` command or `WinSCP` software or `Globus`


3. **Create a PBS Script for Finetuning**  
   The following PBS script schedules a job to finetune Stable Diffusion using the `train_text_to_image_lora.py` script provided by Hugging Face’s `diffusers` library:

   ```bash
   #!/bin/bash
   #PBS -l select=1:ncpus=4:ngpus=1:mem=16GB:gpu_model=V100
   #PBS -l walltime=2:00:00
   #PBS -j oe
   #PBS -N sd-finetune
   #PBS -M <your_email>@unsw.edu.au
   #PBS -m abe

   # Load necessary modules
   module load python

   # Set directories and variables
   export base_dir="/srv/scratch/$USER/finetune"
   export model_name="CompVis/stable-diffusion-v1-4"
   export dataset_name="pranked03/flowers-blip-captions"

   # Set Hugging Face cache to scratch
   export HF_HOME="/srv/scratch/$USER/huggingface_cache"

   # Activate the virtual environment
   source $base_dir/venv_deforum/bin/activate

   # Finetune with the training script
   accelerate launch $base_dir/diffusers/examples/text_to_image/train_text_to_image_lora.py \
     --pretrained_model_name_or_path=$model_name \
     --dataset_name=$dataset_name \
     --resolution=128 --center_crop --random_flip \
     --train_batch_size=32 \
     --gradient_accumulation_steps=1 \
     --gradient_checkpointing \
     --mixed_precision="fp16" \
     --max_train_steps=4000 \
     --learning_rate=1e-05 \
     --max_grad_norm=1 \
     --lr_scheduler="constant" --lr_warmup_steps=0 \
     --output_dir="$base_dir/sd-flower-model" \
     --rank=8 \
     --resume_from_checkpoint="latest"
   ```

4. **Submit the Finetuning Job**  
   Submit the job using:
   ```bash
   qsub <pbs_script_name>.pbs
   ```

5. **Monitor Progress**  
   Monitor the progress of your training job using your HPC’s job management commands, such as [`qstat` or `squeue`](../../using_katana/monitor_jobs) (depending on the HPC system).

---

**Important Reminder:**  
Since the scratch directory is **not backed up**, it is essential to **move any important files** (e.g., your trained models) to your home directory after finetuning. You can use Katana OnDemand or following command to copy files from scratch to your home directory:
```bash
cp -r /srv/scratch/$USER/finetune/sd-flower-model /home/$USER/
```

This ensures that your models are safely stored.

## Video Tutorial on Deforum

This video is a summary of the demo presenting the workflow of experimenting with [Deforum](https://github.com/deforum-art/deforum-stable-diffusion) on Katana
<iframe src="https://unsw.sharepoint.com/sites/ResearchTechnologyTraining/_layouts/15/embed.aspx?UniqueId=9d044957-ad7e-4e06-a352-29493aba1900&embed=%7B%22ust%22%3Atrue%2C%22hv%22%3A%22CopyEmbedCode%22%7D&referrer=StreamWebApp&referrerScenario=EmbedDialog.Create" width="640" height="360" frameborder="0" scrolling="no" allowfullscreen title="Running and fine-tuning GenAI Stable diffusion on Katana HPC cluster.mp4"></iframe>
