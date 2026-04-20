title: Ollama

<h1>Using Ollama on Katana</h1>
Ollama is a lightweight tool for running large language models locally on your machine (no cloud required). It lets you download, run, and interact with models like LLaMA-style chat models through a simple CLI or API.

Ollama is available on Katana as an environment module.
You do not need to install it manually.

This guide shows a simple workflow for running Ollama on Katana.

<h2>Minimal Working Workflow</h2>

```bash
qsub -I -l select=1:ncpus=4:mem=32gb:ngpus=1 

module load ollama 

export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models 

mkdir -p $OLLAMA_MODELS 

ollama serve &> ollama.$(date +%s).log & 

ollama pull phi3 

ollama run phi3 

To exit type /exit 
```

------------------------------------------------------------------------

<h2>1. Start an Interactive Job</h2>

Ollama should run on a compute node, not a login node.

<h3>GPU job (recommended)</h3>

```bash
qsub -I -l select=1:ncpus=4:mem=32gb:ngpus=1
```

<h3>CPU job (for small models)</h3>

```bash
qsub -I -l select=1:ncpus=4:mem=16gb
```

After the job starts, you will see something like:

```bash
z1234567@k001:~
```

This means you are now on compute node k001.

------------------------------------------------------------------------

<h2>2. Load the Ollama Module</h2>

```bash
module load ollama
```

Verify the Ollama Installation

After loading the module, check that Ollama is available:

```bash
module load ollama

ollama --version
```

You may see a warning message similar to:
```bash
Warning: could not connect to a running Ollama instance
Warning: client version is 0.17.7
```
------------------------------------------------------------------------

<h2>3. Set Model Storage Location (Recommended)</h2>

Models are large, so store them in scratch instead of home. On Katana, home directories are quota-controlled and not intended for storing very large model files; using scratch keeps downloads fast and avoids filling your home directory. Set `OLLAMA_MODELS` to a scratch path and create that directory before pulling models:

```bash
export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models
mkdir -p $OLLAMA_MODELS
```

------------------------------------------------------------------------

<h2>4. Start the Ollama Service</h2>

```bash
export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models
module load netsandbox
safe_ollama
```

Expected output:

```bash
Ollama serve starting, log file at ollama.xxxx
```

```bash
ollama list
```

------------------------------------------------------------------------

<h2>5. Recommended Method: Open Another Terminal via SSH </h2>

This is the recommended way to run multiple commands while keeping the
server running.

Open a new terminal (CMD / PowerShell / Terminal).

------------------------------------------------------------------------

Step 1 — SSH to Katana login node

```bash
ssh zID@katana.restech.unsw.edu.au
```

------------------------------------------------------------------------

Step 2 — SSH to the same compute node

Use the node name shown earlier.

Example:

```bash
z1234567@k001:~
```

Then run:

```bash
ssh k001
```

Now you are connected to the same running job.

This allows you to control Ollama while the server keeps running.

------------------------------------------------------------------------

Verify you are on the correct node

```bash
hostname
```

Expected:

```bash
k001
```

------------------------------------------------------------------------

<h2>Start Using Ollama</h2>

Pull a model

```bash
ollama pull phi3
```
The phi3 model is recommended for first-time users because it is small and runs reliably on both CPU and GPU sessions.It is a small language model developed by Microsoft.  
It is designed to be fast, efficient, and easy to run on standard hardware.

Run the model

```bash
ollama run phi3
```
After a couple seconds:
```bash
>>>Send a message (/? for help)
```
Now you can start chat with the model by typing into terminal.

------------------------------------------------------------------------

Useful Commands
Exit current model:

```bash
/exit
```

List models:

```bash
ollama list
```

Show running models:

```bash
ollama ps
```

Stop Ollama:

```bash
pkill ollama
```

------------------------------------------------------------------------

<h2>Ending the Session</h2>

When finished:

```bash
pkill ollama
exit
```

This will:

-   stop Ollama
-   release the compute resources

------------------------------------------------------------------------

GPU vs CPU Recommendations

CPU-friendly models

    phi3
    gemma:2b
    tinyllama

Recommended GPU models

    llama3
    mistral
    gemma:7b

Large models (require high VRAM)

    llama3:70b
    mixtral

------------------------------------------------------------------------

