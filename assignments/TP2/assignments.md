## Exercise 1: Job Arrays

Create config.txt that contains the table below:
```bash
cat config.txt

ArrayTaskID   SampleName        Sex
1             Ahmed             M
2             Amine             M
3             Amal              F
4             Samir             M
5             Ali               M
6             Camelia           F
7             Myriam            F
8             Radouan           M
9             Ikram             F
10            Sara              F
```

Write batch file using ```--array``` command that reads this config file and stores the output below to ```output.txt```.

```bash
$ cat output.txt 
This is array task 2, the sample name is Amine and the sex is M.
This is array task 1, the sample name is Ahmed and the sex is M.
This is array task 3, the sample name is Amal and the sex is F.
This is array task 4, the sample name is Samir and the sex is M.
This is array task 5, the sample name is Ali and the sex is M.
This is array task 7, the sample name is Myriam and the sex is F.
This is array task 6, the sample name is Camelia and the sex is F.
This is array task 9, the sample name is Ikram and the sex is F.
This is array task 8, the sample name is Radouan and the sex is M.
This is array task 10, the sample name is Sara and the sex is F.

```

N.B: Limit the number of tasks run at once to 2.

Write batch file using ```--array``` command that reads this config file and stores the output below to ```output.txt```.

```bash
$ cat output.txt 
This is array task 3, the sample name is Amal and the sex is F.
This is array task 5, the sample name is Ali and the sex is M.
This is array task 7, the sample name is Myriam and the sex is F.**
```

## Exercise 2: Job Dependencies

Create two Python functions:
- First one: Calculates the discriminant of a second-degree equation.
- Second one: Prints the roots depending on the discriminant calculated by the first function
  
Create two batch jobs: Job A and Job B. Job B should depend on Job A's completion.

Use the **--dependency** flag to specify the dependency relationship between the jobs.

Submit both jobs and ensure Job B only starts after Job A completes.

## Exercise 3: Resource Check Script Before Job Submission

In this exercise, you will create a Slurm script that checks the available resources (CPU, GPU, and node availability) before submitting a computational job to the cluster. The purpose of this exercise is to ensure that jobs are not submitted when insufficient resources are available, thus avoiding long wait times in the queue.
Requirements:

- Write a script that checks:
    - The available CPUs and GPUs across the nodes.
    - The state of nodes (e.g., idle, reserved, mixed).
    - The availability of GPUs on the GPU partition.

The script should check whether there are enough available resources (based on user-specified minimums) before proceeding with job submission. If resources are insufficient, it should print a message and exit without submitting the job.

### Instructions:

- Check Available Resources:
    - Use the sinfo and squeue commands to check the state of the nodes and the availability of GPUs and CPUs.
    - Parse the output of these commands to identify whether there are enough idle CPUs and GPUs available.

- These values should be configurable by the user through script arguments or environment variables.
