<h1 align="center">Module commands</h1>

### Display the installed GCC versions:
- In simlab:
```shell
module avail -t 2>&1 | grep -i '^\(gcc\)'
```
- Output:
```shell
gcc/7.2.0
GCC/8.3.0
GCC/9.3.0
GCC/10.2.0
GCCcore/8.3.0
GCCcore/9.3.0
GCCcore/10.2.0
```

### Load a module (default one)

```shell
module load GCC
```
### List the loaded modules

```shell
module list
```
- Output:
```shell
Currently Loaded Modulefiles:
 1) GCCcore/10.2.0   2) zlib/1.2.11-GCCcore-10.2.0   3) binutils/2.35-GCCcore-10.2.0   4) GCC/10.2.0  
```
### Unload module
```shell
module unload binutils/2.35-GCCcore-10.2.0
```
- Output (after module list):
```shell
Currently Loaded Modulefiles:
 1) GCCcore/10.2.0   2) zlib/1.2.11-GCCcore-10.2.0   3) GCC/10.2.0  
```
### Unload all modules
```shell
module purge
```
- Output (after module list):
```shell
No Modulefiles Currently Loaded
```
### Modules conflict handling
**In simlab:** 
- Load default GCC version
```shell
module load GCC
```
- Now try to load another GCC version

```shell
module load GCC/9.3.0
```
```shell
WARNING: GCC/9.3.0 cannot be loaded due to a conflict.
HINT: Might try "module unload GCC" first.
```
```shell
module unload GCC
```
- Try again to load GCC/9.3.0
```shell
module load GCC/9.3.0
```
```shell
WARNING: GCCcore/9.3.0 cannot be loaded due to a conflict.
HINT: Might try "module unload GCCcore" first.
```
***Now GCCcore/9.3.0 create the conflict because when loading GCC/10.2.0, GCCcore/10.2.0 is loaded.***

***The same conflicts will be raised for all dependent packages***

***So, all modules finishing by `10.2.0` and `GCCcore-10.2.0` should be unloaded. In that case:***
- GCCcore/10.2.0, zlib/1.2.11-GCCcore-10.2.0, binutils/2.35-GCCcore-10.2.0 and GCC/10.2.0

***The best solution is to `purge` all modules.***


### Add modules to `~/.bashrc` file

- Open `~/.bashrc` file, and add the modules that you load most of the time

```shell
module load Python/3.8.2-GCCcore-9.3.0 
```
```shell
source ~/.bashrc
```
***You no longer need to load Python/3.8.2-GCCcore-9.3.0, after each connection.***
