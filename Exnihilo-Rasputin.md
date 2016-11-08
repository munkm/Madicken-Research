### Entry List
### Entry Links: ###

* [Installing TPLs] (#TPLinstall)
* [Building Exnihilo](#exnihilobuild)
* [Running Tests in Exnihilo](#exnihilotest)
* [The base.cmake file](#cmakebase)
* [the base.sh file](#shellbase)


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


### Running Tests in Exnihilo [exnihilotest]###

To run the tests in exnihilo, navigate to the build directory. 

### Full Explanation of base.cmake [cmakebase]###

### Full Explanation of base.sh [shellbase]###

