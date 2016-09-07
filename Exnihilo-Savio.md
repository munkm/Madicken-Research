### Entry List
### Entry Links: ###

* [Installing TPLs] (#TPLinstall)
* [Building Exnihilo](#exnihilobuild)
* [Running Tests in Exnihilo](#exnihilotest)
* [The base.cmake file](#cmakebase)
* [the base.sh file](#shellbase)
* [Syncing origin/master](#syncing)


***

### Installing the Third Party Libraries [TPLinstall]###

Exnihilo has build scripts that configure all of the TPLs and in a way that is compatible
with the build. For the TPLs on Savio, we have tried a variant of builds. In general, the 
two extremes are:
* Building with all of Savio's TPLs
* Building all of the TPLs from source with Exnihilo's build scripts. 

First I will describe how to build all of the TPLs, then I will describe how to use Savio's
pre-built TPLs instead.

The location of each configuration file for the TPLs is under `Exnihilo/install/codes/`. 
In this folder there are several shell scripts (`.sh`) that start with `install_`. If you 
open one of these scripts, you can see the configuration options that are enabled
or disabled for that build. For example, the `install_ATLAS.sh` file looks like:

```
#! /bin/sh

# note: "-Fa alg -fPIC" appends the -fPIC flag to all C compilers

lapack_path=$(ls -1 ${source_base}/lapack*.tgz | tail -n 1)

if [ -z "${lapack_path}" ]; then
  echo "LAPACK .tgz should be downloaded to ${source_base}"
  exit 1
fi

if [[ "${sys}" == "remus" ]]; then
  ARCH_FLAGS="-A AMDDOZER64AVXFMA4"
fi

${source_dir}/configure \
  -b 64 -C ic gfortran \
  -Fa alg -fPIC \
  ${ARCH_FLAGS} \
  --with-netlib-lapack-tarfile=${lapack_path} \
  --shared \
  --prefix=${prefix_dir} \
|| exit $?

if [ "${no_build}" == "true" ]; then exit 0; fi

# Specify actual cache sizes
if [[ "${sys}" == "remus" ]]; then
  echo "48" > tune/sysinfo/res/L1CacheSize
  echo "1000" > tune/sysinfo/res/L2CacheSize
  echo "16384" > tune/sysinfo/res/L3CacheSize
fi

#make -j${num_procs} || exit $?
make -j1 || exit $?
${sudo_cmd} make install || exit $?
```

In general, you shouldn't be editing the install_ files, unless your system is very particular.
Most of the edits necessary for savio have already been incorporated. 

The intsall_  files are for general installations. For more specific installs, you might 
note that there are several folder options in the `/codes/` directiory. Of note are
 LAVA and HDF5. If you navigate to one of these folders, you'll see a few variant files for the build.
 For example, HDF5 contains a base.cmake and a parallel.cmake file. To build hdf5 without
 parallel support, then, you'd navigate back to the /install/ folder and execute `./install.sh hdf5`.
 To invoke the parallel support, you'd instead execute `install.sh hdf5 parallel`

### Building Exnihilo [exnihilobuild]###

If all of the TPLs are available, you can build exnihilo. Building exnihilo is done with the
install scripts in the `/install/rc` and `install/codes/Exnihilo/` folders. Unlike the TPLS,
Exnihilo builds can be done with many different customization options. 

** On an interactive node: **

On savio, you can't build using a logon node. You must use an interactive node because
rsync (what copies the files in a build) is disabled. I
detail my process figuring this out in my lab notebook, but I'll go through the
quick-and-dirty way of doing that here. 

Essentially, to do an interactive build you execute:
```
srun --pty --partition=savio --time=02:00:00 bash -i
```

* srun is the slurm command to run a program
* --partition=savio tells slurm to use a compute note on savio
* --pty gives you a pseudo-terminal that can run bash. If you don't use --pty you won't have
access to things like vim or tab autocompletion, which can be *really* annoying. 
* --time= is the time allotmnet that you're giving yourself for this process. 2 hours should
be more than enough for a build, but if you're debugging this you might run out of time.
* bash is the program that you're calling with srun
* -i asks for an interactive node

You can also add more flags to this command. For example:
* -n number will request a specific number of tasks with which you can run. If you want 
some parallelized features this might be relevant.

Once you log on to an interactive node, source the relevant software to build exnihilo,
then execute:

```
./install.sh Exnihilo VARIANT
```
Other relevant information:

* Savio's interactive nodes have $USER@n00%d-- so unless the install.sh file is edited, 
this won't build on the "savio" system, because the build script looks for a "ln00" system.
I have edited the install.sh file on master to look for n and ln nodes (compute and logon
 nodes), but if you branch from somewhere else this might not be in your build scripts. 


Resources:
https://yunmingzhang.wordpress.com/2015/06/29/how-to-use-srun-to-get-an-interactive-node/

Note: the SLURM documentation changed between when I originally learned how to use
interactive nodes. The first link includes the exact info I needed, but the resources below
are the updated ones where this information isn't as clear. You can peruse them at your own
leisure. 

https://wiki.duke.edu/display/SCSC/SLURM+Queueing+System
https://computing.llnl.gov/tutorials/linux_clusters/
https://computing.llnl.gov/tutorials/linux_clusters/index.html#Starting
https://computing.llnl.gov/linux/slurm/srun.html
http://research-it.berkeley.edu/services/high-performance-computing/running-your-jobs



### Running Tests in Exnihilo [exnihilotest]###

### Full Explanation of base.cmake [cmakebase]###

### Full Explanation of base.sh [shellbase]###

### Syncing origin/master [syncing]###

Pulling updated branches from origin (which should be one of ORNLs servers) isn't as 
straightforward on a machine like savio like it might be on your local machine. There are
a few important notes:

* Exnihilo was originally cloned using Rachel's account logged into logon node 001. This means
once you logon to savio, you must ssh to ln001 for things to work correctly. 
  * This also means that you must tunnel to ornl-login1 from ln001 
* Make sure after you pull that you use the script in the top directory to reset the permissions 
of all of the files to match the code profile read/write permssions. If you don't, then
you will be the only one that's able to write files in the new directory. This script is
located at /global/home/groups/ac_nuclear/exnihilo/fix-access.sh

An example of shelling to ln001 after logging onto a different logon node on savio:
```
madmunk@Rasputin:~ $ ssh savio
Password:
------------------------------------------------------------------------------

           B E R K E L E Y    R E S E A R C H    C O M P U T I N G


Research IT at the University of California, Berkeley, operates this computer
system for faculty, staff, and students of the UCB campus.

Additional details about the BRC High-Performance Computing program are
available at:
  http://research-it.berkeley.edu/services/high-performance-computing

Contact Information:
  http://research-it.berkeley.edu/services/high-performance-computing/contact

Support email address:
  brc-hpc-help@lists.berkeley.edu

------------------------------------------------------------------------------
madmunk@ln003:~ $ ssh ln001
Password:
Last login: Tue Aug 16 20:45:58 2016 from 73.162.101.178
------------------------------------------------------------------------------

           B E R K E L E Y    R E S E A R C H    C O M P U T I N G


Research IT at the University of California, Berkeley, operates this computer
system for faculty, staff, and students of the UCB campus.

Additional details about the BRC High-Performance Computing program are
available at:
  http://research-it.berkeley.edu/services/high-performance-computing

Contact Information:
  http://research-it.berkeley.edu/services/high-performance-computing/contact

Support email address:
  brc-hpc-help@lists.berkeley.edu

------------------------------------------------------------------------------
madmunk@ln001:
```

the fix-access.sh file contents:
```
madmunk@ln001: $ cat /global/home/groups/ac_nuclear/exnihilo/fix-access.sh
#! /bin/sh
chgrp -Rf savio_exnihilo $1
chmod -Rf 0770 $1
```

**A workaround:** 

If, for whatever reason, somebody fetched or pulled and didn't update the permissions in 
this folder properly, then you won't be able to pull because you'll get some issue
with the write permissions in the .git subfolder of /src/. There is a workaround for this.
The .git subfolder has folders within it that correspond to each tracking branch. Origin is 
likely what you've pulled from, and likely the subfolder that is having permissions issues.
Since we know the address of the remote tracking branch origin (`ssh://angband/repos/git/Exnihilo.git`),
I created a second remote tracking branch called angband. When I had this issue I could then
pull and synch master using angband rather than origin. This looks like:

```
$ git remote show origin
$ git remote add angband ssh://angband/repos/git/Exnihilo.git
$ git fetch angband
$ git rebase angband/master
```
