# ATTENTION

This is a quick tutorial, please make sure to read the user guide first!

[User guide (PT-BR)](https://www.cenapad.unicamp.br/portal-do-usuario/guia/)

## CENAPAD Tutorial

To connect to CENAPAD, use SSH:

```sh
ssh <user>@cenapad.unicamp.br -p <port>
```

This will put you on the frontend machine, from which you can access the Lovelace cluster. Do not save files on this machine, instead, use the Lovelace environment.

```sh
ssh lovelace
```

Here, you can download the exercises and use the provided scripts to execute them. Note that you will not run the exercises on this machine directly. Instead, you will launch the execution to the cluster queue, which will execute and output the result to a file.

## Example 1

It is recommended that you test the cluster environment with your existing OpenMP programs. To do so, you can use the `openmp-matmul-cenapad.pbs` script in this repository. Simply clone your OpenMP labs and put this .PBS file inside the `01-OmpFor-MatMul` folder. Then, run `qsub 01-OmpFor-MatMul`.

### Example 2

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

If you check the `matsum-cenepad.pbs` file, you can see that it looks like a shell script with a header that instructs how it should be executed on a cluster.

```sh
#PBS -N matsum            # Job name
#PBS -q testegpu          # Queue to execute
#PBS -e job_output.out    # Program standard output
#PBS -o job_output.err    # Program standard error output
#PBS -l walltime=00:25:00 # Timeout to kill the job
```

After that, there is a section that loads some environment modules:

```sh
module load gcc/9.4.0
module load cmake/3.21.3-gcc-9.4.0
module load cuda/11.5.0-gcc-9.4.0
```

Since a cluster has many users, it uses environment modules to allow many GCC versions to coexist, for example. In this case, GCC 9.4.0 is being loaded.

Finally, the script uses CMAKE and make to build the code, just like you would on your machine. After that, it executes the code and compares the output with the expected values.

You can see the queue with `qstat`, or use `qstat -u $USER` to show your jobs only.

To check the output, you can read the `job_output` files:

```sh
cat job_output.out
cat job_output.err
```

