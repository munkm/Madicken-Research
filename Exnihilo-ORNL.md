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


### Using ORNL Servers [usingservers]###


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



