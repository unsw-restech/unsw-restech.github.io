title: Ollama

<h1>Using Ollama on Katana</h1>

Ollama is available on Katana as an environment module.
You do not need to install it manually.

This guide shows a simple workflow for running Ollama on Katana.

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

Check installation:

```bash
ollama --version
which ollama
```

------------------------------------------------------------------------

<h2>3. Set Model Storage Location (Recommended)</h2>

Models are large, so store them in scratch instead of home.

```bash
export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models
mkdir -p $OLLAMA_MODELS
```

------------------------------------------------------------------------

<h2>4. Start the Ollama Service</h2>

```bash
ollama serve
```

Expected output:

```bash
Listening on 127.0.0.1:11434
```

At this point, the terminal is occupied by the Ollama server.

You now need another terminal to run commands like:

```bash
ollama pull
ollama run
```

------------------------------------------------------------------------

Recommended Method: Open Another Terminal via SSH

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

Run the model

```bash
ollama run phi3
```

------------------------------------------------------------------------

Useful Commands

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

<h2>Minimal Working Workflow</h2>

Terminal 1:

    qsub -I -l select=1:ncpus=4:mem=32gb:ngpus=1

    module load ollama

    export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models

    ollama serve

Terminal 2:

    ssh zID@katana.restech.unsw.edu.au

    ssh k001

    module load ollama

    export OLLAMA_MODELS=/srv/scratch/$USER/ollama/models

    ollama pull phi3

    ollama run phi3
