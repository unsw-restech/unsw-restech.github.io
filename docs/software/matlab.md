title: Matlab

## Running interactively

Interactive sessions of matlab are best run on [Katana OnDemand](../using_katana/ondemand.md). 


## Batch Jobs

You can run matab within a batch job. The example below shows the flags used to start matlab without a graphical interface. The matlab script (scriptfile.m) needs to be in the same directory as qsub was run to submit the batch job. 

``` bash
   matlab -nodisplay -nosplash -r scriptfile
```

If you wish to submit the job from a different directory than your matlab script, you need to provide the full file path to cd (change directory) command with matlab. Note the use of quotes.

``` bash
matlab -nodisplay -nosplash -r "cd('/path/to/script/');scriptfile"
```

Later versions of matlab provide the '-batch' flag as an alternative. 

``` bash
matlab -batch -r scriptfile
```
