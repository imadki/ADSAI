## I. Check the available resources using `sinfo` and `squeue` commands
- It's recommended to check the available resources before submitting a job to be sure that your job won't be waiting/pending.

### 1. `sinfo` command
- Display all partitions information
```shell
sinfo
```
- Output in Simlab
```shell
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
defq*        up    1:00:00      2   resv node[01-02]
defq*        up    1:00:00      3    mix node[03-04,14]
defq*        up    1:00:00      1  alloc node15
defq*        up    1:00:00      1   idle node05
gpu          up 2-00:00:00      2    mix node[06,14]
gpu          up 2-00:00:00      9  alloc node[07-10,12-13,15-17]
gpu          up 2-00:00:00      1   idle node11
shortq       up    4:00:00      2   resv node[01-02]
shortq       up    4:00:00      3    mix node[03-04,14]
shortq       up    4:00:00      1  alloc node15
shortq       up    4:00:00      1   idle node05
longq        up 30-00:00:0      2   resv node[01-02]
longq        up 30-00:00:0      3    mix node[03-04,14]
longq        up 30-00:00:0      1  alloc node15
longq        up 30-00:00:0      1   idle node05
visu         up 1-00:00:00      1    mix visu01
special      up      30:00      2   resv node[01-02]
special      up      30:00      4    mix node[03-04,06,14]
special      up      30:00      9  alloc node[07-10,12-13,15-17]
special      up      30:00      2   idle node[05,11]
```

<table>
  <tr>
    <th>Partition</th>
    <th>Max. Cpu Time</th>
    <th>Nodes available for the partition</th>
  </tr>
  <tr>
    <td>defq</td>
    <td>1 hour</td>
    <td>7 (node[01-05], node14, node15)</td>
  </tr>
  <tr>
    <td>shortq</td>
    <td>4 hours</td>
    <td>7 (node[01-05], node14, node15)</td>
 </tr>
  <tr>
    <td>longq</td>
    <td>30 days</td>
    <td>7 (node[01-05], node14, node15)</td>
  </tr>
  <tr>
    <td>special</td>
    <td>30 minutes</td>
    <td>17 (all nodes)</td>
  </tr>
  <tr>
    <td>visu</td>
    <td>24 hours</td>
    <td>1 (visu01)</td>
  </tr>
  <tr>
    <td>gpu</td>
    <td>48 hours</td>
    <td>12 (node[06-17])</td>
  </tr>
</table>


- Display the state for each node 
```shell
sinfo -o "%n %G %C %t"
```
- Output in Simlab
```shell
HOSTNAMES GRES CPUS(A/I/O/T) STATE
node01 (null) 0/40/0/40 resv
node02 (null) 0/40/0/40 resv
node03 (null) 30/14/0/44 mix
node04 (null) 30/14/0/44 mix
node14 gpu:1 43/1/0/44 mix
node15 gpu:1 44/0/0/44 alloc
node05 (null) 0/44/0/44 idle
node06 gpu:1 6/38/0/44 mix
node07 gpu:1 44/0/0/44 alloc
node08 gpu:1 44/0/0/44 alloc
node12 gpu:1 44/0/0/44 alloc
node13 gpu:1 44/0/0/44 alloc
node16 gpu:1 44/0/0/44 alloc
node09 gpu:1 0/44/0/44 idle
node10 gpu:1 0/44/0/44 idle
node11 gpu:1 0/44/0/44 idle
node17 gpu:1 0/44/0/44 idle
visu01 gpu:1 1/43/0/44 mix
```
- Display the available CPUs per partition in Simlab
```shell
sinfo -o "%n %G %C %t %P" --noheader | grep -v -e "resv" -e "drain" -e "maint" | awk '{split($3,cpus,"/"); partition[$5]+=cpus[2]} END {for (p in partition) print p, partition[p]}'
```
- Output in Simlab:
```shell
special  438
gpu      366
shortq   94
defq*    94
visu     44
longq    94
```

***You can run `cpusinfo` command to get the same output***

### 2. `squeue` command

- Display information for all jobs
```shell
squeue
```
- Output:
```shell
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5858549       gpu     bash ikissami PD       0:00      1 (Resources)
           5857887     longq     Toth zakaria.  R 8-21:16:45      1 node15
           5858432     longq jupyter- haitham.  R   22:25:01      1 node14
           5858338     longq    ST_fn    safae  R 1-19:00:29      1 node14
           5858335     longq   300_fn    safae  R 1-20:13:32      1 node14
           5858334     longq 2_str_10    safae  R 1-20:18:16      1 node14
           5858548       gpu     bash ikissami  R       3:09      1 node13
           5858546       gpu     bash ikissami  R       3:37      1 node16
 [...]
 ```

- Display only your jobs information
```shell
squeue -u $USER
```
- Output
```shell
 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5858549       gpu     bash ikissami PD       0:00      1 (Resources)
           5858548       gpu     bash ikissami  R       4:04      1 node13
           5858546       gpu     bash ikissami  R       4:32      1 node16
```

- Display a specific job information
```shell
squeue -j 5858548
```
- Ouput:
```shell
 JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5858548       gpu     bash ikissami  R       4:26      1 node13
```
- Display the pending jobs
```shell
squeue -u $USER -t pending
```
- Output:
```shell
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
           5858549       gpu     bash ikissami PD       0:00      1 (Resources)
```
### 3. Useful commands (CPUs & GPUs availability)
- Display the available GPUs 
    - In simlab there is only one GPU per node
```shell
sinfo -o "%n %G %C %t"
```
```shell
HOSTNAMES GRES CPUS(A/I/O/T) STATE
node01 (null) 0/40/0/40 resv
node02 (null) 0/40/0/40 resv
node03 (null) 30/14/0/44 mix
node04 (null) 30/14/0/44 mix
node14 gpu:1 22/22/0/44 mix
node15 gpu:1 44/0/0/44 alloc
node05 (null) 0/44/0/44 idle
node06 gpu:1 1/43/0/44 mix
node07 gpu:1 1/43/0/44 mix
node13 gpu:1 1/43/0/44 mix
node16 gpu:1 6/38/0/44 mix
node10 gpu:1 44/0/0/44 alloc
node12 gpu:1 44/0/0/44 alloc
node08 gpu:1 0/44/0/44 idle
node09 gpu:1 0/44/0/44 idle
node11 gpu:1 0/44/0/44 idle
node17 gpu:1 0/44/0/44 idle
visu01 gpu:1 0/44/0/44 idle
```
***This command does not give exact information about the available GPUs***

***When the state is mixed for a node containing gpu, it does not mean that the gpu is not available***

**Example:**
- Allocate 1 task in the gpu partition
```shell
srun --partition=gpu --nodelist=node11 --pty bash
```
- Run `sinfo -o "%n %G %C %t" | grep node11`
```shell
node11 gpu:1 1/43/0/44 mix
```
- Now I can run `srun --partition=gpu --nodelist=node11 --gres=gpu:1 --pty bash` to allocate the gpu in the `node11`
- Run `sinfo -o "%n %G %C %t" | grep node11`
```shell
node11 gpu:1 2/43/0/44 mix
```

- In Simlab Add this command to `~/.bashrc` file, then tap `source ~/.bashrc`
```shell
gpuspernode=1
cpuspernode=44
alias gpusinfo='(squeue -t RUNNING -o "%N %b %C" | awk "NR>1 {split(\$2, gpuArray, \":\"); nodes[\$1]+=\$2; gpus[\$1]+=gpuArray[2]; cpus[\$1]+=\$3} END {for (node in nodes) print node, '$gpuspernode'-gpu\
s[node], '$cpuspernode'-cpus[node]}" && sinfo -p gpu --states=idle --noheader -o "%n %G %c" | grep -v -e "maint" -e "drain" -e "resv" | awk "{gsub(/[^0-9]/, \"\", \$2); print \$1, \$2, \$3}") | grep -F "\
$(sinfo -o "%n %G" | grep "gpu" | awk "{print \$1}")" | column -t'
```

- Display the information about GPU availability
```shell
gpusinfo
```

- Output in Simlab
```shell
node07  0  43
node16  1  39
node10  0  0
node12  0  0
node13  0  43
node14  0  22
node06  0  43
node15  1  0
node08  1  44
node09  1  44
node11  1  44
node17  1  44
```
***This means that node08, node09, node11, node16 and node17 are available to be allocated***


## II. Slurm Parameters: `--time`, `--array` and `--constraint`

### 1. `--time`
  - *max runtime for job (required); format: days-hours:minutes:seconds (days- is optional)*
    - `--time=24:00:00`   *# max runtime 24 hours (same as `--time=1-00:00:00`)*
    - `--time=7-00:00:00` *# max runtime 7 days*
    - In Simlab, the time limite depends on the partition
   
- Display the time limite for all jobs
```shell
squeue -l
```
- Output:
```shell
Wed Oct  9 10:02:14 2024
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
           5876585       gpu   my_job youness.  PENDING       0:00 2-00:00:00      1 (QOSMaxGRESPerUser)
           5876589    shortq exo3_cor fatimaza  RUNNING       0:01     10:00      1 node14
           5876583       gpu     bash youness.  RUNNING      11:03 2-00:00:00      1 node17
           5876487       gpu   SPATIO ameck.do  RUNNING 1-22:25:14 2-00:00:00      1 node14
           5876505      visu     visu sboughou  RUNNING   21:29:19 1-00:00:00      1 visu01
           5876495       gpu jupyter- hamsbaai  RUNNING 1-11:34:39 1-20:10:00      1 node16
```
**Example:**

```shell
#!/bin/bash

#SBATCH --partition=gpu # partition name
#SBATCH --nodes=1  # number of nodes to reserve
#SBATCH --gres=gpu:1

sleep 100 
```

```shell
Wed Oct  9 10:10:00 2024
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
           5876595       gpu time.slu ikissami  RUNNING       0:02 2-00:00:00      1 node14
```

***If you want to modify the time limit***

**Solution:**

- Cancel all the jobs and add `--time` option to limite the time
	- `scancel JOBID`
	- `scancel -u <username>`
	- `scancel -t PENDING -u <username>`

```shell
#!/bin/bash

#SBATCH --partition=gpu # partition name
#SBATCH --nodes=1  # number of nodes to reserve
#SBATCH --gres=gpu:1 
#SBATCH --time=04:00:00 # max runtime 4 hours

sleep 100 
```

- Run the jobs
```shell
sbatch batch_gpu.slurm
```

```shell
Wed Oct  9 10:07:40 2024
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
           5876592       gpu time.slu ikissami  RUNNING       0:06   4:00:00      1 node14
```

- Or, update the time limit for the current jobs using `scontrol` commad.

```shell
scontrol update jobid=5876595 Timelimit=04:00:00
```


### 2. `--array`

- Array jobs are good to use when you have multiple samples each of which can utilize an entire compute node running software that
supports multiple threads but does not support MPI
	- `--array=0-23` # job array indexes 0-23
	- `--array=1-24` # job array indexes 1-24
	- `--array=1,3,5,7` # job array indexes 1,3,5,7
	- `--array=1-7:2` # job array indexes 1 to 7 with a step size of 2 do not use `--nodes` with `--array`
 
- Use the index value to select which commands to run either from a text file of commands or as part of the input file name or parameter value
	- `$SLURM_ARRAY_TASK_ID` is the array index value

- stdout and stderr files can be saved per index value
	- `--output=stdout.%x.%A_%a`
	- `--error=stderr.%x.%A_%a`
- maximum array size is 1000 and max total pending and running jobs per user is 1000
	- Limit the number of simultaneously running tasks
	- can help prevent reaching file and disk quotas due to many intermediate and temporary files
	- as one job completes another array index is run on an available node
	- `--array=1-40%5` # job array with indexes 1-40; max of 5 running array jobs

**Example:**

```shell
#!/bin/bash
#SBATCH --jobname=pi_estimation 
#SBATCH --partition=gpu 
#SBATCH -n 6
#SBATCH --array=1-6

# Load python module                                                                                                                                                                                       module load Python/3.8.2-GCCcore-9.3.0

# Define the array of num_samples                                                                                                                                                                           
num_samples_array=(1000 10000 100000 1000000 10000000 100000000)

# Get the num_samples for this task
num_samples=${num_samples_array[$SLURM_ARRAY_TASK_ID-1]}

# Run the python script
python -c "
import random                                                                                                                                                                                              def estimate_pi(num_samples):                                                                                                                                                                                  num_inside_circle = 0
    for _ in range(num_samples): 
        x = random.uniform(-1, 1) 
        y = random.uniform(-1, 1) 
        distance = x**2 + y**2
                                                                                                                                                                                                            
        if distance <= 1: 
            num_inside_circle += 1
            
    return 4 * num_inside_circle / num_samples 
print(estimate_pi($num_samples))
"
```
- After `squeue -u $USER`
```shell
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
         5859016_1       gpu pi_estim ikissami  R       0:00      1 node14
         5859016_2       gpu pi_estim ikissami  R       0:00      1 node14
         5859016_3       gpu pi_estim ikissami  R       0:00      1 node13
         5859016_4       gpu pi_estim ikissami  R       0:00      1 node13
         5859016_5       gpu pi_estim ikissami  R       0:00      1 node13
         5859016_6       gpu pi_estim ikissami  R       0:00      1 node16
```

### 3. `--constraint`
- The `--constraint` option in Slurm is used to specify hardware or feature constraints for the nodes on which the job should run
- In Simlab for example, you can specify the type of the GPU (V100, P40) when asking resources:
	
**Example:**
```shell
srun --partition=gpu --nodes=1 --constraint=V100 --gres=gpu:1 --pty bash
```

### 4. `--dependency`

- You may submit jobs that runs depending on status of the previously submitted jobs or schedule a bunch of jobs to run one after the other.

- Once you submit a job, using that job ID, you can submit dependency jobs.

- You need to extract the job id “12345” from the output of the “sbatch” command
```shell
$ sbatch job.cmd
Submitted batch job 12345
```
- By adding the “–parsable” option to “sbatch command”, only the job ID would be returned and its value can be stored in a shell variable for later use.

```shell
$ jobID=$(sbatch --parsable job.cmd)
$ echo ${jobID}
12345
```
- Next, you can submit a job that only runs after successful completion of the first job as follows where we set the “afterok” as the dependency type.

```shell
$ sbatch --dependency=afterok:${jobID} second_job.cmd
```

The format here is
```shell
$ sbatch --dependency=type:job_id jobfile
```

- If the job requires more than one job to be completed before it is executed, you can supply all the jobids using , separator

```shell
$ sbatch --dependency=type:job_id,job_id,job_id jobfile
```

- You can also set the job to run if any one of the job ids completes successfully using a ? separator
```shell
$ sbatch --dependency=type:job_id?job_id?job_id jobfile
```

- The other dependencies that can be used for<type:job_id> are as follows:

| **Type**      | **Description**                                                                                  |
|---------------|--------------------------------------------------------------------------------------------------|
| `after`       | This job can begin execution after the specified jobs have begun execution.                       |
| `afterany`    | This job can begin execution after the specified jobs have terminated.                            |
| `aftercorr`   | A task of this job array can begin execution after the corresponding task ID in the specified job has completed successfully. |
| `afternotok`  | This job can begin execution after the specified jobs have terminated in some failed state.       |
| `afterok`     | This job can begin execution after the specified jobs have successfully executed.                 |
| `singleton`   | This job can begin execution after any previously launched jobs sharing the same job name and user have terminated |


#### Example:

- dependency.slurm

```shell
#!/bin/bash                                                                                                                                                                                                                     

# Submit the first job and capture its job ID                                                                                                                                                                                   
job1=$(sbatch --parsable job1.slurm)
echo "Submitted job1 with Job ID: $job1"

# Submit the second job, dependent on the successful completion of job1                                                                                                                                                         
job2=$(sbatch --dependency=afterok:${job1} --parsable job2.slurm)
echo "Submitted job2 with Job ID: $job2, dependent on job1"

# Submit the third job, dependent on the successful completion of job2                                                                                                                                                          
job3=$(sbatch --dependency=afterok:${job2} --parsable job3.slurm)
echo "Submitted job3 with Job ID: $job3, dependent on job2"
```

- job1.slum:
```shell
#!/bin/bash                                                                                                                                                                                                                     

#SBATCH --job-name=example_job    # Job name                                                                                                                                                                                    
#SBATCH --output=job1_output.txt  # Standard output and error log                                                                                                                                                               
#SBATCH --error=job1_error.txt    # Separate file for error log (optional)                                                                                                                                                      
#SBATCH --ntasks=1                # Run a single task                                                                                                                                                                           
#SBATCH --time=00:05:00           # Time limit (hh:mm:ss)                                                                                                                                                                       
#SBATCH --mem=100M                # Memory limit                                                                                                                                                                                

# Commands to execute                                                                                                                                                                                                           
echo "Hello, World!"
```

- job2.slum:
```shell
#!/bin/bash                                                                                                                                                                                                                     
#SBATCH --job-name=sum_numbers     # Job name                                                                                                                                                                                   
#SBATCH --output=job2_output.txt   # Standard output log                                                                                                                                                                        
#SBATCH --error=job2_error.txt     # Error log                                                                                                                                                                                  
#SBATCH --ntasks=1                 # Run a single task                                                                                                                                                                          
#SBATCH --time=00:05:00            # Time limit (hh:mm:ss)                                                                                                                                                                      
#SBATCH --mem=100M                 # Memory limit                                                                                                                                                                               

# Commands to execute                                                                                                                                                                                                           
sum=0
for i in {1..100}; do
  sum=$((sum + i))
done

echo "The sum of numbers from 1 to 100 is: $sum"
```


- job3.slum:
```shell
#!/bin/bash
#SBATCH --job-name=write_date      # Job name
#SBATCH --output=job3_output.txt   # Standard output log
#SBATCH --error=job3_error.txt     # Error log
#SBATCH --ntasks=1                 # Run a single task
#SBATCH --time=00:05:00            # Time limit (hh:mm:ss)
#SBATCH --mem=100M                 # Memory limit

# Commands to execute
echo "Current date and time: $(date)"
```

- Run the job

```shell
sbatch dependency.slurm
```
