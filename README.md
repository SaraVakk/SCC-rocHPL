# Instructions for installing and running rocHPL!
1. Go to working folder "sara" (you should find rocHPL from there already installed, if not follow steps 2-12)
   ### If rocHPL directory already exists skip to step 9!!!
2. install:
```git clone https://github.com/ROCm/rocHPL.git```
3. ```cd rocHPL```
4. create following bash script in nano, name: rochpl_bashscript: 
```
#!/bin/bash
#Build inside subshell:
(
        module load cce
        module load PrgEnv-cray
        module load craype-x86-rome
        module cray-mpich
        module load craype-accel-amd-gfx90a

        mpi_root=$CRAY_MPICH_DIR

        # Use cray wrapper:
        export CXX=CC
        export HIPCXX=CC

       ./install.sh --with-mpi=$mpi_root 
)
```

5. build script:
   ```bash rochpl_bashscript```
6. create runscript for cray compiler:
  ```nano hplrunscript```
7. add the script to created file from https://github.com/cschpc/student-cluster-competition-2024/blob/main/benchmarks/rocHPL/run_rochpl_cce
8. edit necessary bits: line 85 ```rochpl_bin=FIXME```, replace fixme with realpath of rochpl binary. (Navigate to build/bin/rochpl.version_number and print the realpath)
9. Try to run using:  
    ```MPICH_GPU_SUPPORT_ENABLED=1 srun --exclusive --nodes=1 --ntasks-per-node=2 --account=sara --time=00:15:00 --mem=0 ./hplrunscript -P 1 -Q 2 -N 128000 --NB 512```

10. If permission denied check rights:  
  ```ls -l /home/sara/rocHPL/hplrunscript``` (change path if working in different environment)
11. Change permissions:  
 ```chmod +x /home/sara/rocHPL/hplrunscript``` (again, change path if necessary)
12. And run the script from command line  
    For 1 node:  
    ```MPICH_GPU_SUPPORT_ENABLED=1 srun --exclusive --nodes=1 --ntasks-per-node=2 --account=sara --time=00:15:00 --mem=0 ./hplrunscript -P 1 -Q 2 -N 128000 --NB 512```  
    For 3 nodes:  
    ....  
    NOTE: --cpu-bind=none not supported in cluster. Assining cpu's and gpu's did not work due to gres settings.
   