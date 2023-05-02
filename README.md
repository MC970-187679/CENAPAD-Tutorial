# ATTENTION

This is a quick tutorial, make sure to read the user guide first!

[User guide (PT-BR)](https://www.cenapad.unicamp.br/portal-do-usuario/guia/)

## CENAPAD Tutorial

Connect to CENAPAD using SSH:

```sh
ssh <user>@cenapad.unicamp.br -p <port>
```

This will put you in the frontend machine, from which you will access the Lovelace cluster. Do not save files in this machine, instead we will use the Lovelace environment.

```sh
ssh lovelace
```

Here, you can download the exercises and use the provided scripts to execute. Note that you will note run the exercises in this machine directly, instead, you will launch the execution to the cluster queue. It will then execute and output the result to a file.

### Example

Clone the repository:

```sh
git clone --recurse-submodules <repository URL>
```

Enter a lab repository:

```sh
cd CUDA-exercises/01-MatSum/
```

Launch the execution:

```sh
qsub matsum-cenapad.pbs
```

If you check the `matsum-cenepad.pbs` file, you can see that it looks like a shell script, with a header that instructs how it should be executed on a cluster.

```sh
#PBS -N matsum            # Name of the job
#PBS -q testegpu          # Queue to execute
#PBS -e job_output.out    # Program std output
#PBS -o job_output.err    # Program stderr output
#PBS -l walltime=00:25:00 # Timeout to kill the job
```

After that, we have a section that loads some environment modules:

```sh
module load gcc/9.4.0
module load cmake/3.21.3-gcc-9.4.0
module load cuda/11.5.0-gcc-9.4.0
```

Since a cluster have many users, it uses envinronment modules to allow many GCC versions to co-exist, for example. In our case, we are loading GCC 9.4.0, for example.

Finally, the script uses CMAKE and make to build the code, just like you would in your machine.
After that, it executes the code and compares the output with the expected values.

You can see how is the queue with `qstat`, or use `qstat -u $USER` to show your jobs only.

To check the output, you can read the files `job_output`:

```sh
cat job_output.out
cat job_output.err
```
