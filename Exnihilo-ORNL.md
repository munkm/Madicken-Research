### Entry List
### Entry Links: ###

* [Using ORNL servers](#usingservers)
* [Notes from Tara](#taradirs)
* [Installing TPLs](#TPLinstall)
* [Building Exnihilo](#exnihilobuild)
* [Running Tests in Exnihilo](#exnihilotest)
* [The base.cmake file](#cmakebase)
* [the base.sh file](#shellbase)
* [Syncing origin/master](#syncing)


***

### Summary for Madicken [summarymunk]###


Cloning:
git clone ssh://angband/repos/mirror/advantg.git
git clone ssh://angband/repos/git/Exnihilo.git
git clone ssh://angband/repos/git/Exnihilo.git Exnihilo2
git clone ssh://angband/repos/mirror/TriBITS.git
git clone ssh://angband/repos/mirror/Trilinos.git
git clone ssh://angband/repos/mirror/SCALE.git
git clone ssh://angband/repos/mirror/lava.git

1.) ssh ornl-login1 (login using pin+RSA key)
2.) ssh remus (login using ucams password)
3.) make sure .bashrc on user profile contains the following:
```
# Sets up Exnihilo path configuration
source /projects/exnihilo/rc

# Points to MCNP executable
PATH=$PATH:/projects/MCNP5_v1.6/MCNP_CODE/bin

# Exnihilo dbc install
export PATH=/home/m15/install/Exnihilo/bin:${PATH}
export PYTHONPATH=/home/m15/install/Exnihilo/python:${PYTHONPATH}
export LD_RUN_PATH=/home/m15/install/Exnihilo/lib:${LD_RUN_PATH}

# advantg install
export PATH=/home/m15/install/advantg/bin:${PATH}
export PYTHONPATH=/home/m15/install/advantg/lib/python2.7/site-packages:${PYTHONPATH}
export LD_RUN_PATH=/home/m15/install/advantg/lib:${LD_RUN_PATH}
```
4.) make the following directories:
mkdir your_install_dir/advantg
mkdir your_install_dir/Exnihilo
mkdir tmp/${USER}/advantg
mkdir tmp/${USER}/Exnihilo

5.) execute the following: 
```
$ ./install.sh -c /home/m15/src/Scale/ -b /tmp/munk-build/Exnihilo -p 
/home/m15/install/Exnihilo Exnihilo for-advantg
$
$ ./install.sh -c /home/m15/src/advantg -b /tmp/munk-build/advantg -p /home/m15/install/advantg advantg
$
```

6.) Navigate to the build directory. Then go to the exnihilo build directory and execute: `$ make test`
Make sure all of the tests pass. 

7.) Navigate to the advantg build directory. Execute `$ make check`, then execute `$ make test`.
Make sure all tests pass. 






### Using ORNL Servers [usingservers]###

To access any of the ORNL git repos stored on angband, or to log on to Remus, Romulus, or
any of the ORNL clusters, you first need to log in to login1. 

STEP 1:
ssh ornl-login1


### Notes from Tara [taradirs]###


First, put this in your bashrc:
```
                source /projects/exnihilo/rc-advantg
```

1)       Get an interactive job (minus uppercase i, then minus lowercase l): 
```
qsub –I -l nodes=1;ppn=32 –l walltime=1:00:00
```

2)       Make a directory in /tmp like : 
```
mkdir /tmp/munk-build/
```

3)       Clone Exnihilo and ADVANTG.  And probably Trilinos and TriBITS.  IF you need SCALE, 
clone that too or use the skeleton version you have.

4)       Get on your branches on Exnihilo and ADVANTG.

5)       Update the install scripts to turn off and on what you want.  I would make new files
 called munk or something else. They are located here:

/Exnihilo_source/install/codes/advantg/remus/

6)       Add to your bashrc to point to your install paths.  Check out Seth’s bashrc to see some Advantg stuff:

 

# Sets up Exnihilo, python, gcc, openmpi, MCNP DATA paths
```
source /projects/exnihilo/rc-advantg
```
 

# Exnihilo dbc build
```
export PATH=/YOUR_INSTALL_PATH/bin:${PATH}

export PYTHONPATH=/YOUR_INSTALL_PATH/python:${PYTHONPATH}

export LD_RUN_PATH=/YOUR_INSTALL_PATH/lib:${LD_RUN_PATH}
```
 
```
PATH=$PATH:/projects/MCNP5_v1.6/MCNP_CODE/bin      
```
 

7)       Install!

Go into the Exnihilo source code: Exnihilo_source/install/

 

In the following command I am assuming you saved your cmake file as munk.cmake.  
This command will install a Release build.  You can also do a debug

 

Execute for just Exnihilo: 

./install.sh –c /path_to_your_Advantg_source/ -b /tmp/munk-build/ –p /path_to_where_you_want_to_install/ -t munk advantg


### Installing TPLs [#TPLinstall]###

To build Exnihilo on Remus, you do not need to install the TPLs. 
