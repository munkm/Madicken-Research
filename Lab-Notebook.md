### Entry List
### Entry Links: ###

* [Entry: 2017/02/25](#entry-20170225)
* [Entry: 2017/02/24](#entry-20170224)
* [Entry: 2017/02/23](#entry-20170223)
* [Entry: 2017/02/22](#entry-20170222)
* [Entry: 2017/02/21](#entry-20170221)
* [Entry: 2017/02/17](#entry-20170217)
* [Entry: 2017/02/16](#entry-20170216)
* [Entry: 2017/02/09](#entry-20170209)
* [Entry: 2016/03/16](#entry-20160316)
* [Entry: 2016/03/09](#entry-20160309)
* [Entry: 2016/03/08](#entry-20160308)
* [Entry: 2016/03/07](#entry-20160307)
* [Entry: 2016/03/04](#entry-20160304)
* [Entry: 2016/02/16](#entry-20160216)
* [Entry: 2016/02/11](#entry-20160211)
* [Entry: 2016/02/10](#entry-20160210)
* [Entry: 2015/12/15](#entry-20151215)
* [Entry: 2015/12/04](#entry-20151204)
* [Entry: 2015/12/03](#entry-20151203)
* [Entry: 2015/12/02](#entry-20151202)
* [Entry: 2015/12/01](#entry-20151201)
* [Entry: 2015/11/30](#entry-20151130)
* [Entry: 2015/11/25](#entry-20151125)
* [Entry: 2015/11/23](#entry-20151123)
* [Entry: 2015/11/22](#entry-20151122)


***

### Entry: 2016/09/22

Omnibus update:

Commit from Seth

>>    Merge branch 'omnibus-frontend'
>>
>>    This is a rewrite of the Omnibus input processing. It vastly improves
>>    error reporting, inter-parameter dependency checking, and documentation.
>>
>>    Some defaults may have changed or no longer be present. Most notably are
>>    that the PN order and number of MC particles (for fixed-source and
>>    kcode) no longer have defaults. Default reporting is more consistent and
>>    intelligent.
>>
>>    A few parameters, databases, and types have been renamed. The parameters
>>    and databases use a new ``Deprecated`` entry to mark that they've
>>    changed and report them to the user, allowing old inputs to run.
>>
>>    Another major change on the input is that inapplicable parameters are
>>    now reported errors, with a detailed message about why they are not
>>    applicable. This prevents users from unintentionally providing input
>>    that isn't used (e.g. a ``[SOURCE]`` block in a kcode Denovo-only
>>    problem). The applicability of parameters is now improved, particularly
>>    for source options.
>>
>>    The ``[RUN=titan]`` mode is now ``[RUN=cray]`` to support execution of
>>    Denovo on other Cray supercomputers that use aprun (namely the DOD
>>    Excalibur cluster). The way defaults are chosen has changed for all the
>>    PBS clusters, especially for Titan. I recommend
>>
>>    For developers, the biggest change is in defining new aspects to the
>>    Omnibus input schema (parameters, defaults, etc.). Let's get together if
>>    you want me to go over how these work. I have also added initial hooks for
>>    postprocessing options.
>>
>>    The ASCII writer is currently offline due to the way the output is being
>>    processed. When preprocessing you will find a '.inp.json' that contains
>>    the processed options.

My method is in omnibus, so this may cause some issues. 

***

### Entry: 2016/03/16

Step 1: comment out the hdf5 lib paths in rc/savio/base.cmake
1a: also change HDF5 base dir path to the module path base location
1b: change the base, prefix and build directory to the TPL builds
Step 2: load all new modules and the savio hdf5
Step 3: rebuild silo with this hdf5, rather than the one built by me.
step 4: add hdf5 parallel to the module-list.sh file i made for module loading
step 5: login to interactive node
step 6: try to build exnihilo


first i tried these steps, but first skipping step 3 and using the silo built with my version of hdf5, but using the savio pre-built hdf5 to install savio.

```
/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib/libhdf5_hl.a(H5LT.o): In function `H5LT_dtype_to_text':
H5LT.c:(.text+0x4443): undefined reference to `H5Tget_member_value'
H5LT.c:(.text+0x5089): undefined reference to `H5Tget_tag'
H5LT.c:(.text+0x5186): undefined reference to `H5Tget_cset'
H5LT.c:(.text+0x53fc): undefined reference to `H5Tset_cset'
H5LT.c:(.text+0x5500): undefined reference to `H5Tset_cset'
/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib/libhdf5_hl.a(H5LTparse.o): In function `H5LTyyparse':
H5LTparse.c:(.text+0x190): undefined reference to `H5Tget_native_type'
H5LTparse.c:(.text+0x25d): undefined reference to `H5Tenum_insert'
H5LTparse.c:(.text+0x2e8): undefined reference to `H5Tenum_insert'
H5LTparse.c:(.text+0x36b): undefined reference to `H5Tenum_insert'
H5LTparse.c:(.text+0x3ee): undefined reference to `H5Tenum_insert'
H5LTparse.c:(.text+0x46e): undefined reference to `H5Tenum_insert'
H5LTparse.c:(.text+0x500): undefined reference to `H5Tenum_create'
H5LTparse.c:(.text+0x652): undefined reference to `H5Tset_cset'
H5LTparse.c:(.text+0x751): undefined reference to `H5Tset_tag'
make[2]: *** [Exnihilo/packages/Nemesis/database/test/Nemesis_tstDB_HDF5.exe] Error 1
make[1]: *** [Exnihilo/packages/Nemesis/database/test/CMakeFiles/Nemesis_tstDB_HDF5.dir/all] Error 2
[ 16%] Built target Nemesis_tstDB2PL_Converter
make: *** [all] Error 2
```



second i tried to build silo with savio's hdf5. this was my error:
```
mv -f .deps/silock.Tpo .deps/silock.Po
/bin/sh ../../libtool --tag=CXX   --mode=link /global/software/sl-6.x86_64/modules/langs/intel/2015.6.233/bin/intel64/icpc  -fPIC  -L/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib  -Wl,-rpath -Wl,/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib -o silock silock.o ../../src/libsiloh5.la  -lhdf5  -lm
libtool: link: /global/software/sl-6.x86_64/modules/langs/intel/2015.6.233/bin/intel64/icpc -fPIC -Wl,-rpath -Wl,/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib -o silock silock.o  -L/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib ../../src/.libs/libsiloh5.a /global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib/libhdf5.so -lz -ldl -lm -Wl,-rpath,/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib -Wl,-rpath,/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/lib
../../src/.libs/libsiloh5.a(silo_hdf5.o): In function `db_hdf5_Open':
silo_hdf5.c:(.text+0x1011): undefined reference to `H5Pset_fapl_mpiposix'
silo_hdf5.c:(.text+0x1ca1): undefined reference to `H5Pset_fapl_mpiposix'
../../src/.libs/libsiloh5.a(silo_hdf5.o): In function `db_hdf5_process_file_options':
silo_hdf5.c:(.text+0x238b): undefined reference to `H5Pset_fapl_mpiposix'
silo_hdf5.c:(.text+0x264f): undefined reference to `H5Pset_fapl_mpiposix'
../../src/.libs/libsiloh5.a(silo_hdf5.o): In function `db_hdf5_Create':
silo_hdf5.c:(.text+0x2caa): undefined reference to `H5Pset_fapl_mpiposix'
# User name
../../src/.libs/libsiloh5.a(silo_hdf5.o):silo_hdf5.c:(.text+0x346e): more undefined references to `H5Pset_fapl_mpiposix' follow
make[3]: *** [silock] Error 1
make[3]: Leaving directory `/global/scratch/munkm/TPLbuilds/silo/tools/silock'
make[2]: *** [all-recursive] Error 1
make[2]: Leaving directory `/global/scratch/munkm/TPLbuilds/silo/tools'
make[1]: *** [all-recursive] Error 1
make[1]: Leaving directory `/global/scratch/munkm/TPLbuilds/silo'
make: *** [all] Error 2
``` 

According to some of my googling, mpi posix was removed in hdf5 1.8.13, so i'm not sure how to deal with this. 
References:
* http://www.unidata.ucar.edu/mailing_lists/archives/netcdfgroup/2014/msg00188.html
* https://www.hdfgroup.org/HDF5/doc/RM/RM_H5P.html#Property-SetFaplMpiPosix
* https://github.com/Unidata/netcdf-c/issues/63
* http://hdf-forum.184993.n3.nabble.com/Re-undefined-reference-to-H5Pset-fapl-mpiposix-td4027216.html

what I find really weird is that the version of hdf5 that I build on my own is also 1.8.16, just like the one i'm trying to use that's pre-built on savio. I have no idea why this error would occur with savio's version, but not mine. 

### Entry: 2016/03/09

#### Static library stuff: ####

Seth has recommended that exnihilo be built with static libraries

There are a few ways we can do this: 
* explicitly link to the .a lib files in the /codes/Exnihilo/savio/base.cmake file
* SET(SHARED_LIBRARIES OFF CACHE BOOL)
* I do both just to be redundant in the cmake file. 

Before I was having issues with some linking of the static blas/lapack libraries. The error looked like this:
```
Trilinos/packages/thyra/core/src/CMakeFiles/thyracore.dir/support/nonlinear/model_evaluator/client_support/Thyra_DefaultFiniteDifferenceModelEvaluator.cpp.o
Building CXX object Trilinos/packages/thyra/core/src/CMakeFiles/thyracore.dir/support/nonlinear/model_evaluator/client_support/Thyra_DirectionalFiniteDiffCalculator.cpp.o
/global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/liblapack.a(ilaenv.o): In function `ilaenv':
/global/software/sl-6.x86_64/sources/lapack-3.6.0/SRC/ilaenv.f:204: undefined reference to `for_cpystr'
/global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/liblapack.a(xerbla.o): In function `xerbla':
/global/software/sl-6.x86_64/sources/lapack-3.6.0/SRC/xerbla.f:90: undefined reference to `for_len_trim'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/SRC/xerbla.f:90: undefined reference to `for_write_seq_fmt'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/SRC/xerbla.f:90: undefined reference to `for_write_seq_fmt_xmit'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/SRC/xerbla.f:92: undefined reference to `for_stop_core'
[ 15%] /global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/liblapack.a(iparmq.o): In function `iparmq':
```

Seth says:
>It's because that BLAS version implicitly requires linking against a fortran library (typically libgfortran.a); but since it's not a shared library (which explicitly tells the linker what else to link against), and since it's being linked into C++ code (where libfortran isn't automatically linked), it's failing. (Notice how all the missing symbols start with "for_"?) So basically, you'll have to find the right libfortran and include it as a library alongside the blas library

So to set the fortran library to be linked alongside the lapack library, you modify the base.cmake file's lib variable like this:
`SET(TPL_BLAS_LIBRARIES "/global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/libblas.a;BLAHBLAH/libfortran.a" CACHE PATH "") `

Seth said that the library should look like libgfortran.a, but I noticed that was the *gcc* version of the fortran library, not the intel version. 

* Navigating around in the intel folder, i found a series of library files in `/global/software/sl-6.x86_64/modules/langs/intel/2015.6.233/bin/intel64/`
* None of the intel .a files had fortran explicitly in it. So i looked up what the library files meant from this link:
  * http://www2.units.it/divisioneisi/ci/tartaglia/intel/fce/main_for/mergedProjects/bldaps_for/files_32.htm
  
* Unfortunately, I've linked against every single one of these libraries and I am still getting compile issues. Sigh. 


#### Building Silo with static libraries: ####

When i build silo with the build scripts provided, a .a file is never created. 
I got around this by making this loop in the install_silo.sh file in the install scripts:
```
if [ "${sys}" == "savio" ]; then
  SHARED_ARGS="--disable-shared --enable-static"
fi
```


### Entry: 2016/03/08

* still debugging savio build. Having issues with static libraries. Silo isn't even building a static library. sigh. 



### Entry: 2016/03/07

#### Build Notes: ####

For some reason, I can't build on the head node anymore. Seth says that this is because rsync (what copies and pastes files) is disabled. 
* This used to be enabled
* It isn't available now
* I am building on an interactive node by executing 
```
srun --pty --partition=savio --time=02:00:00 bash -i
```
  and then loading my environment file after logging in to the new node.
* Savio's interactive nodes have $USER@n00%d-- so unless the install.sh file is edited, this won't build on the "savio" system, because the build script looks for a "ln00" system. 
  * I modified the install.sh to inclue the interactive nodes too. 
* I turned off lava, since explicit MCNP linking is called by Lava, so I could try to diagnose other errors without having the mcnp executable issues. 

With shared libraries turned off, and building on an interactive node, i've been running into a few issues:
* unless explicitly linking to a .a LAPACK or HDF5 library, the build stuff links to .so files
  * The errors look like this:
  ```
  [ 14%] Building CXX object Trilinos/packages/thyra/core/src/CMakeFiles/thyracore.dir/support/operator_vector/adapter_support/Thyra_SpmdVectorDefaultBase.cpp.o
[ 14%] Building CXX object Exnihilo/packages/Nemesis/utils/test/CMakeFiles/Nemesis_tstFlat_Table.dir/tstFlat_Table.cc.o
ld: warning: libhdf5.so.10, needed by /global/home/groups/ac_nuclear/TPLs/packages_intel/silo/lib/libsiloh5.so, may conflict with libhdf5.so.10.1.0
ld: warning: libhdf5.so.10, needed by /global/home/groups/ac_nuclear/TPLs/packages_intel/silo/lib/libsiloh5.so, may conflict with libhdf5.so.10.1.0
Linking CXX executable Nemesis_tstAssocVector.exe
[ 14%] Building CXX object Trilinos/packages/thyra/core/src/CMakeFiles/thyracore.dir/support/operator_vector/adapter_support/Thyra_SpmdVectorSpaceDefaultBase.cpp.o
  ```
* If I do explicitly link to the path of the those files, I get cmake errors:
```
Scanning dependencies of target Nemesis_tstHistogram
[ 14%] Building CXX object Exnihilo/packages/Nemesis/utils/test/CMakeFiles/Nemesis_tstHistogram.dir/tstHistogram.cc.o
/global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/libblas.a(xerbla.o): In function `xerbla':
/global/software/sl-6.x86_64/sources/lapack-3.6.0/BLAS/SRC/xerbla.f:80: undefined reference to `for_len_trim'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/BLAS/SRC/xerbla.f:80: undefined reference to `for_write_seq_fmt'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/BLAS/SRC/xerbla.f:80: undefined reference to `for_write_seq_fmt_xmit'
/global/software/sl-6.x86_64/sources/lapack-3.6.0/BLAS/SRC/xerbla.f:82: undefined reference to `for_stop_core'
Linking CXX executable Nemesis_tstEnumerate.exe
[ 14%] Built target Nemesis_tstCXX11_Types
[ 14%] Building CXX object Trilinos/packages/thyra/core/src/CMakeFiles/thyracore.dir/interfaces/operator_solve/fundamental/Thyra_LinearOpWithSolveBase.cpp.o
Scanning dependencies of target Nemesis_tstHyperslab_Vector
[ 14%] Building CXX object Exnihilo/packages/Nemesis/utils/test/CMakeFiles/Nemesis_tstHyperslab_Vector.dir/tstHyperslab_Vector.cc.o
Linking CXX executable Nemesis_tstContainer_Search.exe
make[2]: *** [Exnihilo/packages/Nemesis/utils/test/Nemesis_tstBLAS.exe] Error 1
make[1]: *** [Exnihilo/packages/Nemesis/utils/test/CMakeFiles/Nemesis_tstBLAS.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
Linking CXX executable Nemesis_tstGrid_Lookup.exe
```
* I went through several variations of library calls to try to debug the build today. It still dies at around 6%. Sigh. 

Notes:
* ADVANTG requires python wrappers to run. 
  * I need shared libraries to have the python wrappers. 
  * We can't build with the shared libraries with the savio configuration as it is. 
  * Marissa can still do work without the python wrappers. 
* Seth is working on the ADVANTG interface to be able to call and run advantg without these wrappers.
  * It won't be ready for a few weeks.
  * The workaround is to run advantg on a different system, then copy the denvo runs over to savio for a run. 
  * This is not idea, obviously. 
  * Or we can wait until Seth's thing is ready.  





### Entry: 2016/03/04

####Steps of today's process:####

* committed old install files (those are very old changes, but couldn't be committed due to earlier space issues)
* did a git remote update for origin
* did a git merge origin/master into our local branch
  * There were no conflicts, which is what I expected since nobody should be messing with the savio install files. 
* checked savio's module availability:
  * `module list` shows what is loaded
  * `module avail` shows the available loadable modules
     * in the case of python and intel, once one is loaded, executing `module avail` a second time will update the available modules that are dependent on python or intel. 
  * The old modules we had in our bashrc do not exist anymore. I updated these in a new file that we can `source exnihilo_mods`. I put it in the top directory for exnihilo (e.g. `/global/home/groups/ac_nuclear/exnihilo/`, so any user should be able to source it if they need to.  
* I cloned the developer scale from angband into a new folder called `$GRP_DIR/SCALE_dev`. I thought this would be useful if we want to assign it different group permissions that 6.2b. 
  * I cloned this also from ornl-login1, so it won't get confusing if we need to update either package set. 
* Rebuilt TPLS that were necessasry (pcre, swig, silo, and hdf5)
  * modified base.sh to reflect the new TPL locations


####Building with Savio compilers rather than my built TPLs:####
* libraries exnihilo needs: 
  * gcc -- in langs
  * openmpi -- in intel/2015
  * blas/lapack --- in intel/2015
  * hdf5 -- parallel and static in intel/2015
  * lava must build from source
  * pcre -- **not available**???? 
  * swig  -- **not available**???
  * silo -- **not available**???m
  * atlas -- in intel/2015
  * qt -- in modfiles/tools
* I changed the compilers from gcc, g++ and gfortran in the base.sh file to icc, icpc, and ifort. 


####Building the TPLS again:####
I made things simple. First i unloaded the default intel, then I loaded the new intel, the new openmpi, and the new gcc.
Executing `module list` yeilds:

```
Currently Loaded Modulefiles:
  1) intel/2015.6.233      2) openmpi/1.8.8-intel   3) gcc/4.8.5
```

Then I executed the following commands:
* `./install.sh pcre`  -- had no issues
* `./install.sh swig`  -- had no issues
* `./install.sh silo`  -- had issue:
   -- build failed. end of build printed this:
   ```
checking for ftell function pointer... yes
checking for fwrite function pointer... yes
checking for memmove... yes
checking for fnmatch... yes
checking for isnan... yes
checking for fpclass... no
checking for strerror... yes
checking if setjmp and longjmp work... yes
checking for cos in -lm... yes
checking for szlib... suppressed
checking for hdf5... configure: checking for hdf5 and supporting libraries...
configure: error: problem with directory specified for hdf5 includes
   ```
   * Tried loading hdf5. 
   ```
   $ module list
   Currently Loaded Modulefiles:
  1) intel/2015.6.233      3) gcc/4.8.5             5) lapack/3.6.0-intel
  2) openmpi/1.8.8-intel   4) atlas/3.10.2-intel    6) hdf5/1.8.16-intel-s
   ```
   got the same error. 
   * checked $INCLUDE
   ```
   INCLUDE=/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-s/include:/global/software/sl-6.x86_64/modules/intel/2015.6.233/atlas/3.10.2-intel/include:/global/software/sl-6.x86_64/modules/langs/gcc/4.8.5/include:/global/software/sl-6.x86_64/modules/intel/2015.6.233/openmpi/1.8.8-intel/include:/global/software/sl-6.x86_64/modules/langs/intel/2015.6.233/compiler/include
   ```
   * Tried loading parallel hdf5:
   ```
   $ module list
Currently Loaded Modulefiles:
  1) intel/2015.6.233      3) gcc/4.8.5             5) lapack/3.6.0-intel
  2) openmpi/1.8.8-intel   4) atlas/3.10.2-intel    6) hdf5/1.8.16-intel-p
   $ echo $INCLUDE
   global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p/include:/global/software/sl-6.x86_64/modules/intel/2015.6.233/atlas/3.10.2-intel/include:/global/software/sl-6.x86_64/modules/langs/gcc/4.8.5/include:/global/software/sl-6.x86_64/modules/intel/2015.6.233/openmpi/1.8.8-intel/include:/global/software/sl-6.x86_64/modules/langs/intel/2015.6.233/compiler/include
   ```
   
   got the same error. Ugh.
   * Checked for `HDF5 base` string in base.sh file. Added a hardlink to the module folder, rather than letting it assume hdf5 is in my TPL folder. 
   ```
   # HDF5 base path (used by silo install script etc)
export HDF5_BASE_DIR=/global/software/sl-6.x86_64/modules/intel/2015.6.233/hdf5/1.8.16-intel-p
   ```
   * ran `./install.sh silo` again
     * Could be problematic. There were some errors at the end. But no failure in the build.
     * Went to install dir for TPLS `$GRP_DIR/TPLs/build/packages_intel`, silo wasn't there. The install didn't work.
     
   * decided to build hdf5 from source too. 
     * removed hardlink in base.sh to lib path for hdf5 module
     * 
     ```
     ./install.sh hdf5
     export PATH=/global/home/groups/ac_nuclear/TPLs/packages/hdf5/bin:$PATH
     export LD_LIBRARY_PATH=/global/home/groups/ac_nuclear/TPLs/packages/hdf5/lib:$LD_LIBRARY_PATH
     ```
     * then ran `./install.sh silo`
       * there were still some errors at the end of the build, but a folder with /include/ and /lib/ for /silo/ is made in packages_intel. 
* `./install.sh lava` executed after all other TPLS built. A few errors came up, but it seems to have installed.
```
Building C object CMakeFiles/lava.dir/native/spfunc.c.o
[ 78%] [ 78%] [ 80%] [ 80%] Building C object CMakeFiles/lava.dir/native/surface.c.o
Building C object CMakeFiles/lava.dir/native/surfac.c.o
[ 82%] Building C object CMakeFiles/lava.dir/native/torus.c.o
Building C object CMakeFiles/lava.dir/native/track.c.o
Building C object CMakeFiles/lava.dir/native/transform.c.o
In file included from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/global.h(4),
                 from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/newcel.c(5):
/global/home/groups/ac_nuclear/ADVANTG/src/lava/include/lava.h(84): warning #3180: unrecognized OpenMP #pragma
  #pragma omp threadprivate(lava_ErrId, lava_ErrMsg)
          ^

In file included from /global/home/groups/ac_nuclear/ADVANTG/src/lava/include/lava_mcnp5.h(5),
                 from /global/home/groups/ac_nuclear/ADVANTG/src/lava/include/lava.h(117),
                 from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/global.h(4),
                 from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/newcel.c(5):
/global/home/groups/ac_nuclear/ADVANTG/src/lava/include/lava.h(115): warning #3180: unrecognized OpenMP #pragma
  #pragma omp threadprivate(lava_LCG)
          ^

[ 85%] In file included from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/global.h(4),
                 from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/newcel.c(5):
/global/home/groups/ac_nuclear/ADVANTG/src/lava/include/lava.h(129): warning #3180: unrecognized OpenMP #pragma
  #pragma omp threadprivate(lava_State, lava_mState)
          ^

[ 87%] In file included from /global/home/groups/ac_nuclear/ADVANTG/src/lava/native/newcel.c(5):
/global/home/groups/ac_nuclear/ADVANTG/src/lava/native/global.h(25): warning #3180: unrecognized OpenMP #pragma
  #pragma omp threadprivate(lava7l_NeighborLists, lava7l_CellExpr)
          ^

Building C object CMakeFiles/lava.dir/native/uplev.c.o
```


####Configuring Exnihilo for the new build:####
* I updated the `install/rc/savio/base.sh` and the `install/codes/Exnihilo/savio/base.cmake` files to not have paths to my old TPL builds
* I added new symlinks to the `$GRP_DIR/SCALE_dev` folder for Exnihilo, Tribits and Trilinos
* I updated the `base.sh` file to turn on static libraries (at Seth's instruction)
* I coped the `scale-debug` and `debug_insilico.cmake` files into the `/codes/Exnihilo/savio/` folder. They both enable insilico; the former enables ALL of the denovo packages, the latter just enables insilico. 
  * To build with one of these, we will execute `$ ./install.sh Exnihilo scale-debug` or `$ ./install.sh Exnihilo debug-insilico`
  * Before I was building with `$ ./install.sh Exnihilo noscale`. This is not the case any more, so I needed to make sure the right variables were added to whatever variant we wanted to consider. 

####Rebuilding Exnihilo with Insilico support:####

**Attempt 1:**
- `./install.sh Exnihilo insilico_debug`
   * some interesting things that come up in early build steps:
   ```
   Explicitly enabled packages on input (by user):  Nemesis Robus Transcore Geometria Physica Denovo Shift Hyas Insilico Omnibus 10

Explicitly enabled SE packages on input (by user):  Nemesis Robus Transcore Geometria Physica DenovoPyKBA Denovo Shift Hyas InsilicoDepletion Insilico Omnibus 12

Explicitly disabled packages on input (by user or by default):  Sacado Zoltan Xpetra Domi Claps Galeri Pamgen SEACAS Trios Zoltan2 TriKota STK Phdmesh NOX MueLu Aristos ForTrilinos PyTrilinos WebTrilinos NewPackage Optika MeshingGenie Cobra GenData AFIDS GQuery MavricUtilities Poseidon 28

Explicitly disabled SE packages on input (by user or by default):  KokkosExample Sacado Zoltan Xpetra Domi Claps Galeri Pamgen SEACAS Trioscommsplitter Triossupport Triosnnti Triosnssi Triosprograms Triosexamples Triostests Triosnetcdf-service Trios Zoltan2 ShyLUIChol ShyLUGTS TriKota IntrepidIntrepid2 STKExp STK Phdmesh NOX MueLu Aristos ForTrilinos PyTrilinos WebTrilinos NewPackage Optika MeshingGenie Cobra GenData AFIDS GQuery GeometriaDagMC GeometriaBRLCAD DenovoKBA_CUDA DenovoAMP_coupling ShiftMC_sensitivity MavricUtilities SequenceCoupleWrapper SequenceCsasShift SequenceOrsen Poseidon AmpxGNDEndf Ampxxsusa Ampxangular GuiOrigenGui 53
   ```
   * Build failed because it couldn't find BLAS libraries (I commented these out earlier because I figured the module system would update everything just fine)
   * Remedy: Update BLAS and LAPACK libraries and specify them explicitly in base.cmake file 
     * libblas.a and liblapack.a located at `/global/software/sl-6.x86_64/modules/intel/2015.6.233/lapack/3.6.0-intel/lib/`
     * atlas perhaps also useful to link? 
     
**Attempt 2:**
```
CMake Error at cmake/FindMCNP.cmake:46 (message):
  Couldn't run MCNP at /global/home/groups/ac_nuclear/bin/mcnp5_151.mpi to
  determine version
Call Stack (most recent call first):
  Exnihilo/packages/Geometria/lava/CMakeLists.txt:17 (FIND_PACKAGE)


CMake Error at /global/software/sl-6.x86_64/modules/tools/cmake/3.2.2/share/cmake-3.2/Modules/FindPackageHandleStandardArgs.cmake:138 (message):
  Could NOT find MCNP (missing: MCNP_VERSION_STRING)
Call Stack (most recent call first):
  /global/software/sl-6.x86_64/modules/tools/cmake/3.2.2/share/cmake-3.2/Modules/FindPackageHandleStandardArgs.cmake:374 (_FPHSA_FAILURE_MESSAGE)
  cmake/FindMCNP.cmake:60 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  Exnihilo/packages/Geometria/lava/CMakeLists.txt:17 (FIND_PACKAGE)
```



Other potential issues:

**First:**
a. When I load a module, if it is dependent on another module that module also gets added to my module list
b. gcc 4.8.5 is on Savio. Yay! 
c. My method depends on h5py; I'm not sure if it is required for other Denovo/exnihilo/advantg things
d. When I go through and unload the default very old gcc, and then load the newer gcc 4.8.5 module, I found that if I loaded h5py, a different module of hdf5 was loaded than the one I load with the intel compilers. This hdf5 is actually located in the subfolder of the OLD gcc (4.4.something) module lists. As a result, all of the hdf5 commands that I execute thereafter are using this other version of hdf5, not the hdf5 built with either the gcc that I am using OR with the intel compilers I'm using. 


So.  I don't know if Marissa's stuff uses any python interface, but do you think this is going to be an issue? it seems super weird, but I'm not surprised that some TPL dependency is getting wonky, because the admins didn't build anything new with this new gcc. My solution, I guess, if this is going to be a big problem, is to build a python version unique to my local user and use that for any and all python analysis pertaining to Exnihilo. 

**Second:**

All of the build stuff hardlinks to ac_nuclear rather than co_nuclear. This will have to be redone soon. 
Not sure if we should be using -fpic if we are going to link to static libraries?



### Entry: 2016/02/16

Things we can get rid of on Savio:

First I looked at the total group folder sizes (after you and Max moved the XS dirs):
551M    ADVANTG
609M    ARC
2.0G    CTF
196K    EDIS
469M    LANL
9.0G    MCC
1.4G    MCNP
546M    MOOSE
18M MocDown
77M ORIGEN2
37M PARCS
42G SCALE6.2b
3.0G    TPLs
4.0K    Vera
68K bin
1.5M    examples
4.0G    exnihilo
12M monteburns
82G serpent
41M serpent2
15M serpent2.1.23-icc15.0.0
18M serpent2.1.24-icc
9.7M    serpent2_custom_msr
14M serpent2_extended_versions
34M serpent2_msr
12M serpent2_updates
47M serpent_fixed
4.6M    silo
3.3G software

Of these, I've summarized what I think is deleteable: 

* $GRP_DIR/exnihilo/installs -- Yes. Everything can be deleted. But I would like to wait on the 12/15 until we get a new working build. 
```
$ du -sh *
1.2G    exnihilo_20151203
1.2G    exnihilo_20151215
```
* $GRP_DIR/TPLs/  -- Maybe? This depends on if I can get a working build of Exnihilo with the gcc4.8 compiler that has been added over winter break. Yes, it can be deleted, but I'd like to wait until we can be sure. 
```
$ du -sh *
738M    gcc48src
4.0K    gcc52env.sh
833M    gcc52src
903M    packages
554M    src
``` 
* $GRP_DIR/silo -- No; we need it for ADVANTG
* $GRP_DIR/ADVANTG -- It depends. Do we want the ADVANTG distribution that you tarred from the disc? 
```
539M    C831MNYCP00
7.9M    builds
1.6M    installs
2.2M    src
```
* $GRP_DIR/SCALE6.2b -- Yes; skeleton scales can be deleted (this won't make too much of a difference, though) 
```
1.6M    Scale
42G scale6-2b
0   scale_dev_data
1.6M    skeletonscale
``` 



### Entry: 2016/02/11

#### Things to bring up at meeting: ####

* Weird issue with LATEX in .bashrc file (write18 enabled and only being able to execute with sudo)
* python shared libraries
* git repo commit issues (and not being able to rebuild
* parallel vs. single core hdf5? (is there an option that I should change in source file for that, too? ) 
* 


#### Leftover Issues: ####

1. Why did Exnihilo have trouble loading hdf5 and other python databases? (Maybe a static/dynamic library thing)
2. need to rebuild exnihilo with static libraries (look in Titan install scripts for some version of BUILD_SHARED_LIB OFF) 
3. need to figure out the git issue i'm running into:

```
madmunk@ln003: $ git add rc/savio/base.sh
fatal: Unable to write new index file
madmunk@ln003:$ git commit -m 'updating savio files to include mpirun and mpiexec paths in TPL directory'
fatal: unable to write new_index file
madmunk@ln003: $ git status
# On branch master
# Your branch and 'origin/master' have diverged,
# and have 10 and 2064 different commit(s) each, respectively.
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#	modified:   codes/Exnihilo/savio/base.cmake
#	modified:   rc/savio/base.sh
#
no changes added to commit (use "git add" and/or "git commit -a")
```
4. Why did the Python db issue come up? Is the right python being called?
  * I did some searching, and it looks like we get some interesting stuff going into the actual module folder (in /module/ rather than /modfiles/ for h5py:
  ```
  madmunk@ln002:/global/software/sl-6.x86_64/modules/python/2.7.8/h5py/2.5.0/lib/python2.7/site-packages/h5py-2.5.0-py2.7
-linux-x86_64.egg/h5py $ ls -la
total 7840
drwxrwsr-x 4 yqin software    8192 Dec  7 16:23 .
drwxrwsr-x 4 yqin software    4096 Dec  7 16:23 ..
.
.... one two skip a few ....
.
.
-rwxr-xr-x 1 yqin software  172740 Dec  7 16:23 h5ac.so
-rw-rw-r-- 1 yqin software     279 Dec  7 16:23 h5d.py
-rw-rw-r-- 1 yqin software     713 Dec  7 16:23 h5d.pyc
-rwxr-xr-x 1 yqin software  349627 Dec  7 16:23 h5d.so
-rw-rw-r-- 1 yqin software     280 Dec  7 16:23 h5ds.py
-rw-rw-r-- 1 yqin software     716 Dec  7 16:23 h5ds.pyc
-rwxr-xr-x 1 yqin software  205466 Dec  7 16:23 h5ds.so
-rw-rw-r-- 1 yqin software     279 Dec  7 16:23 h5f.py
-rw-rw-r-- 1 yqin software     713 Dec  7 16:23 h5f.pyc
-rwxr-xr-x 1 yqin software  360372 Dec  7 16:23 h5f.so
-rw-rw-r-- 1 yqin software     280 Dec  7 16:23 h5fd.py
-rw-rw-r-- 1 yqin software     716 Dec  7 16:23 h5fd.pyc
-rwxr-xr-x 1 yqin software   75535 Dec  7 16:23 h5fd.so
-rw-rw-r-- 1 yqin software     279 Dec  7 16:23 h5g.py
.
.
.
.
  ```


#### Extra Notes: ####

These messages came up after my successful parallel test run:
```
>>> Writing silo file to 1 concurrent files using material volume fractions.
::: Writing Denovo HDF5 output
>>> Parallel HDF5 is not enabled; using serial HDF5 with parallel communication.
```

In `/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/
packages/Omnibus/driver/test` , running `ctest -V -R OmnibusDriver_tst_kcode_mg_MPI_1` all of the tests failed (even single core runs) because the MPI module couldn't be found: scrolling through, this is what some of the error looked like:

```
10: ERROR: Could not Plot keff and source convergence
10: EXCEPTION: Traceback (most recent call last):
10:   File "/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/packages/Omnibus/frontend/omnibus/postprocess/manager.py", line 140, in process
10:     pp(self, self.db[db_key])
10:   File "/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/packages/Omnibus/frontend/omnibus/postprocess/kcode.py", line 331, in __call__
10:     self.tallies = load_from_manager(manager)
.
.
.
10:   File "/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/packages/Omnibus/frontend/omnibus/postprocess/kcode.py", line 175, in from_manager
10:     with h5py.File(path, 'r') as f:
10:   File "/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/packages/Nemesis/python/exnihilotools/errormodule.py", line 42, in delayed_error_module
10:     result = import_module(name)
10:   File "/global/software/sl-6.x86_64/modules/langs/python/2.7.8/lib/python2.7/importlib/__init__.py", line 37, in import_module
10:     __import__(name)
10: ImportError: No module named h5py
.
.
.
10:
10: FAILED (errors=1)
10: In tst_kcode_mg.py, overall test result: FAILED
1/1 Test #10: OmnibusDriver_tst_kcode_mg_MPI_1 ...***Failed  Error regular expression found in output. Regex=[overall test result: FAILED] 11.15 sec

0% tests passed, 1 tests failed out of 1

Total Test time (real) =  11.20 sec

The following tests FAILED:
	 10 - OmnibusDriver_tst_kcode_mg_MPI_1 (Failed)
```



#### debugging Exnihilo tests today: ####
logging in to savio:
first execute the following:
```
$ source $TPL_DIR/gcc52env.sh
$ module load cmake
$ module load python 2.7.8
$ module load h5py
```

now you have the correct environmental setup to run Exnihilo

To run the tests on an interactive node, you need to use srun:
[http://research-it.berkeley.edu/services/high-performance-computing/running-your-jobs]
The most basic srun command you use `srun -u bash -i`

srun by itself doesn't have tab autocompletion or acccess to vim, which is really difficult if you're going way down the rabbit hole. You can do this by changing the srun command to `srun --pty --partition=partition_name --time=02:00:00 bash -i` 

The savio documentation (linked above) recommends looking into srun's documentation to fully understand those differences [https://computing.llnl.gov/linux/slurm/srun.html].

Then, once logged in to the interactive node, I navigated to a test folder where the test executables lie (e.g. `/global/scratch/munkm/exnihilo_20151215/Exnihilo-noscale/Exnihilo/
packages/Omnibus/denovo_managers/test`) and just executed the executable:
`./OmnibusDenovo_managers_tstDenovo_ManagerSN.exe`

***

Another issue that I was having on savio was being able to run the parallel jobs. To do this in the interactive node, *at submission* you need to allocate the number of cores you'll be using. You can do this with the flag `--cpus-per-task=2` for 2 nodes. 

Then running the same test as before would explicitly call mpi as:
`mpirun -np 2 OmnibusDenovo_managers_tstDenovo_ManagerSPN.exe`

***

Running `make test` inside an interactive node isn't recommended; this is because it calls instantiations of srun, which can cause some wonky stuff. If we want to use it, it'd probably better to do it in a slurm submission script.

However, you can call the tests with ctest if you have the correct number of nodes allocated:
`ctest -V -R OmnibusDriver_tst_cylmesh_MPI_2`

***

./install.sh Exnhililo-noscale needs to be executed if any of the fundamental structure of Exnihilo (like libraries, linking, solver functionality) changes. 

If, instead, you're just adding or modifying a simple helper tool you can navigate to the build folder and execute
`$ make install`
or 
`$ make install/fast`

`$ make test` builds all of the tests (for the directory you're in and lower) and runs them


### Entry: 2016/02/10

#### debugging Exnihilo tests today: ####
module load cmake
module load python 2.7.8
module load h5py
source $TPL_DIR/gcc52env.sh
srun -u -p savio -t 2:0:0 bash -i
make test isn't a good idea because it calls an mpirun, which makes srun call mpi (a recursive issue)
instead do ctest -V -R OmnibusDriver_tst_kcode_mg_MPI_1
this test fails because it can't find h5 py
ctest -V -R OmnibusDriver_tst_cylmesh_MPI_2
instead just call the executable ./OmnibusDenovo_managers_tstDenovo_ManagerSN.exe
```
>>> Using default azimuthals_octant=2
>>> Using default polars_octant=2
[       OK ] DenovoManagerSNFixedTest.fc_analytic_pt_source (3 ms)
[----------] 4 tests from DenovoManagerSNFixedTest (168 ms total)

[----------] Global test environment tear-down
[==========] 7 tests from 2 test cases ran. (326 ms total)
[  PASSED  ] 7 tests.
In ./OmnibusDenovo_managers_tstDenovo_ManagerSN.exe, overall test result: PASSED
```

instead call mpirun explicitly

mpirun -np 2 OmnibusDenovo_managers_tstDenovo_ManagerSN.exe
```
--------------------------------------------------------------------------
There are not enough slots available in the system to satisfy the 2 slots
that were requested by the application:
  OmnibusDenovo_managers_tstDenovo_ManagerSN.exe

Either request fewer slots for your application, or make more slots available
for use.
--------------------------------------------------------------------------
```


### Entry: 2015/12/15

#### Rebuilding Exnihilo: ####

The other week, I was having issues running the tests in Denovo. Part of this was because I should have been trying to run the tests using an interactive node (and not the head node) on savio (which doesn't have tab autocompletion, which sucks for directory navigation). 
After some navigation with Seth and Tara, we found that the mpiexec was defined in the wrong location (i.e. not in the mpi directory i built). 

From `CMakeCache.txt` in `/global/scratch/munkm/exnihilo-20151203/
```
//Where can one of the /usr/bin/sbatch, mpiexec or mpirun libraries
// be found
MPI_EXEC:FILEPATH=/usr/bin/sbatch

```

At first I thought that Savio's system was defining this path explicitly, but I actually found it defined in a different part of the `/install/codes/Exnihilo/savio/base.cmake` file:

```
# Set up cross compiling options to run unit tests on device
SET(SCALE_HOSTNAME Linux CACHE BOOL "")
SET(SCALE_VERBOSE_CONFIGURE OFF CACHE BOOL "")
SET(MPI_EXEC "/usr/bin/sbatch" CACHE FILEPATH "")

...
...

# Compiler settings
SET(TPL_ENABLE_MPI ON CACHE BOOL "")
SET(MPI_C_COMPILER "which gcc" CACHE STRING "")
SET(MPI_CXX_COMPILER "which g++" CACHE STRING "")
SET(MPI_Fortran_COMPILER "which gfortran" CACHE STRING "")
SET(CMAKE_BUILD_TYPE "RelWithDebInfo" CACHE STRING "")

```

Because this is a cmake option, Tara recommended that I rebuild all of Exnihilo. 
I changed the MPI_EXEC variable to $TPL_DIR/packages/openMPI/bin/, and rebuilt exnihilo. 


#### Running Tests: ####

To submit an interactive job to savio, I used this:

srun -u -p savio -t 2:0:0 bash -i

Then, to run one of the tests, I navigated to the build folder and

***

### Week of 12/07-12/12:

* Spent this week writing. No random updates on Savio or my research. 
* Update the gantt chart for next week's meeting with Rachel. 

***

### Entry: 2015/12/04

* Today I talked with Bob about potential postdocs at ORNL
  * In the next year it seems like the primary postdoc position they'll have is in fusion neutronics. I'm not sure if this would be a good path for me to take.
  * Bob also said they would be adding MS-CADIS into ADVANTG, as well as incorporating ADVANTG into the Exnihilo distribution package. 
  * Other potential postdoc opportunities with ORNL (based on my own research): [Distinguished Fellowships at ORNL](https://www.ornl.gov/careers/distinguished-fellowships)
  
* I also worked with Seth and Tara on trying to debug testing in Denovo. More on that later next week, since I want all of the info in one post. 

***

### Entry: 2015/12/03

* **HIGHLIGHT** I got a full build of Exnihilo in /global/scratch today!!!
* Still dealing with the space issues that started yesterday. 
  * I checked ac_nuclear's space allocation with df -h, and as of last night, we were at 201Gb. This was after I deleted all of the old build information. 
  * When trying to build exnihilo in the $GRP_DIR dir, at the install step I mentioned I had some strange errors during the install step yesterday. That looks like this:
  
  ```
  [100%] Built target Insilico_mocsi
>>> Installing to /global/home/groups/ac_nuclear/exnihilo/installs/exnihilo_20151202/Exnihilo-noscale ...
Install the project...
-- Install configuration: "RelWithDebInfo"
-- Up-to-date: /global/home/groups/ac_nuclear/exnihilo/installs/exnihilo_20151202/Exnihilo-noscale/./FileNameAliases.txt
-- Installing: /global/home/groups/ac_nuclear/exnihilo/installs/exnihilo_20151202/Exnihilo-noscale/lib/libgtest.so.6.2
CMake Error at Trilinos/commonTools/gtest/cmake_install.cmake:48 (file):
  file INSTALL cannot copy file
  "/global/scratch/munkm/exnihilo_20151202/Exnihilo-noscale/Trilinos/commonTools/gtest/libgtest.so.6.2"
  to
  "/global/home/groups/ac_nuclear/exnihilo/installs/exnihilo_20151202/Exnihilo-noscale/lib/libgtest.so.6.2".
Call Stack (most recent call first):
  cmake_install.cmake:41 (include)


make: *** [install/fast] Error 1
 
  ```

Building in /global/scratch/${USER}/ is a good temporary solution, but as pointed out in our group meeting today, /scratch/ is not backed up. However, neither is $GRP_DIR, as mentioned in the [savio documentation](http://research-it.berkeley.edu/services/high-performance-computing/user-guide). 


* Should I make a quick link to useful Savio links? Probably yes. 

***

### Entry: 2015/12/02

#### Disc Write Errors: ####

* After leaving my savio install scripts the same, but only changing the build directory location specified in `/install/rc/savio/base.sh`, I found that only disc write errors occured when I built exnihilo in the ac_nuclear group folder. 
  * I e-mailed yong about this issue:

>>Yong, I tried putting the build directory in /global/scratch/munkm and the build completed. I tried putting the build directory in /global/home/groups/ac_nuclear/exnihilo/ and the build aborts due to disc quota exceeded errors. Having this particular software's build files in the ac_nuclear group is important, because we need multiple users to be able to run the tests, not just me. Are we exceeding a folder size limit by building in this directory? 

> Yes. This means you have run out of space on /global/home/groups/ac_nuclear. Please refer to the aforementioned online doc, we have a 200GB disk quota for this directory as well and it is currently exhausted (just verified). So your group will need to clean some space from this directory before you can put more data into it. 

  * This issue will have to be resolved if we want the build directory to be accessible to more than just one person at a time. 
    * An alternative would be that different users can have their own build directories from which they can run tests. These would be located in /global/scratch/${USER}/exnihilo-build. 

#### Debugging the Exnihilo Build with Tara: ####

* Tara was nice enough to schedule an impromptu meeting with me to try to debug my Exnihilo build. 
  * At the end of last night, I had a build reaching ~100%, but nothing is built in the install directory or the build directory. This is strange. I also found that none of the tests passed, and only the Omnibus tests were built.
    * Tara told me that the Omnibus tests are built separately from the rest of the tests in Denovo. I needed to turn on the tests in Denovo to fix that. 
  * I DID, however, find that I was not building the tests in Denovo. To turn them on I added a line in `install/codes/Exnihilo/savio/base.cmake` to: 
  ```
  SET(SCALE_ENABLE_TESTS   ON  CACHE BOOL   "")
  ```
  * For now I have Silo turned on, Lava turned off, and the libraries for HDF5, BLAS, LPAPACK, and SILO defined explicitly in the same base.cmake file. 
  
        
#### Other Correspondance Things: ####

* Another issue that I'm wondering about is the discrepancy between the -fPIC error and the hdf5 static/dynamic libraries. A few days ago I wrote in this notebook that when I was trying to build Exnihilo that I ran into an hdf5 linking error, and it suggested that I recompile with -fPIC: `/global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against '.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC`

  * I talked to Seth about this, and I asked if HDF5 needed to be compiled with -fPIC in addition to my new build of gcc:
>>> Madicken: Does hdf5 also need to be built with the -fPIC flag? The failure happens when it is linking an hdf5 library. 
>> Seth: Yes. Everything you link against needs -fPIC: HDF5, OpenMPI, GCC, GMP, Silo, the works.
>> Good catch -- I bet that's what's happening.

after a few more exchanges: 
>>> Seth: At this point, you're right that Intel is probably not worth the effort. You might get some performance improvement, but since you're going for workability not performance, that doesn't make so much sense.
>>> So if you have your GCC 5.2 build set up with fPIC, and your TPLs, then I'd say run with that and ditch intel.
>> Madicken: Before I do this, I just want to make sure: Exnihilo should be able to build with gcc 5.2, right? There aren't incompatibility issues? 
> Yes, it builds just fine with GCC 5.2. Again, you just have to make sure that everything up the line is built with -fPIC.  I've done this on so many systems now, that's why I wrote the install.sh script and the detailed install directions on our config page :)

* I have confirmation that I need the -fPIC flag turned on in order for Exnihilo to be built. But every time I've asked for confirmation from Krishna or Yong about this flag on any TPL on Savio, I haven't gotten a response. 
  * I told Yong that I needed -fPIC enabled, and that this was the reason I was going to re-compile all of the TPLs I needed with gcc 5.2 and ditch the intel compilers entirely: 
  
>>> I talked to my colleagues at ORNL, and we agreed that because the -fPIC errors were coming up with HDF5, it was probably because the HDF5 compiled with the intel compilers was not compiled with the -fPIC flag enabled. At this point, knowing that I needed to recompile HDF5 (and probably all of the other TPLs necessary for my build), I decided to compile all of my TPLs with gcc5.2, which had the c++11 support I needed, and ditch the Intel compilers entirely. Originally I did not want to re-compile all of these TPLs, which is why I was trying to stay with using the Intel compilers. 

>> As I mentioned to you in my previous email, you will need to link to
the dynamic libraries not the static libraries.

Yong does have a point here. My error that I originally sent him was pointing to `libhdf5.a` and not `libhdf5.co`. However, as detailed in my previous section debugging with Tara, the errors associated with the static library do not invoke the `recompile with -fPIC` error message. I have no idea why after several e-mails, I still don't have a response about -fPIC on the Savio modules. But I responded with the following:

>In my new build of hdf5 I have linked to the dynamic libraries, and not the static libraries. I can go back to trying to use the intel-compiled hdf5 and using intel to compile my software if you think that HDF5 and the other TPLs were compiled with the -fPIC flag. 
> The reason I say this is because in my new build of HDF5, when I had linked against the static libraries accidentally (like I had done earlier with the Intel hdf5), the error messages did not include the -fPIC message that I copied and pasted in an earlier e-mail from the intel-compiled hdf5. Furthermore, the build got to a completely different location with this new version of HDF5. 

Again, I don't really know if there is something else different in my new HDF5 build other than this enabled -fPIC flag, but I do know what the difference between the static and dynamic library linking errors are, now that I went through that with Tara. 

#### Other Notes from Today ####

**At the hacker within:** 
* PyCharm seems like a good tool for writing python
* ctest is a tool to check on which functions dominate in computational work scripts in time and processing 

* Keep in mind that you should not try to optimize your code until it works and is done. <--- I have an issue with this. 
* Parallelization:
  * PySpark is very easy for parallelization. 
  * Shoc benchmarks our system. 
    * Tells us our bandwith between our memory and CPU (11GB/sec)
    * Max flops (1330 GFlops),
      * So our performance is limited by RAM to CPU, not necessarily our total flops. We need to then focus on how much we can pass between ram and cpu and not as much about optimizing our code. 
      * If we can pull in as much data at once as possible, then we can actually use our flops. 
     
***
 
### Entry: 2015/12/01

#### ac_nuclear to co_nuclear ####

* The TPLS that I built yesterday don't work right now because the group ac_nuclear seems to have moved to co_nuclear. I talked to Katy about it, and theoretically we should be able to move whole folders and preserve functionality in different software, but MPI is throwing errors in the exnihilo build.
* I am not positive that this is due to the switch from ac_nuclear to co_nuclear
  * I checked the savio build scripts (both /install/rc/savio/base.sh and /install/codes/Exnihilo/savio/base.cmake) to ensure that the paths to the TPL Libraries were updated to reflect the new co_nuclear. 
  * I did a recursive grep in the rest of the install directory to make sure that ac_nuclear didn't appear anywhere else. 
  * The build still failed when calling mpicc. Mpicc was found in the new co_nuclear folder, but the build scripts seemed to be looking for a file later on in ac_nuclear. I suspected that this had to do with a hard link that got placed in mpicc when I build it. 
  * I recursively grepped in the TPL directory to make sure ac_nuclear didn't appear. It appears that it did. 
* To fix this I anticipate needing to reinstall all of the TPLs in the ${GRP_DIR}/TPLs, to ensure that their functionality is maintained. 
  * I sent Krishna, Yong, and Aron an e-mail about this and Krishna reinstated the symlink between ac_nuclear and co_nuclear. Krishna said that they would work with us to come up with a good time in 2016 to remove the link.
  * When this link is removed, I will have to recompile all of the TPLs.

#### disc write errors ####

* At the end of 11/30, I had a build of Exnihilo reaching to ~80%. The build then aborted due to disc writing errors. 
```
/tmp/ccLluKcw.s: Assembler messages:
/tmp/ccLluKcw.s:8116728: Fatal error: can't close CMakeFiles/tpetra.dir/Tpetra_CrsMatrix_SerialWrapperNode.cpp.o: Disk quota exceeded
make[2]: *** [Trilinos/packages/tpetra/core/src/CMakeFiles/tpetra.dir/Tpetra_CrsMatrix_SerialWrapperNode.cpp.o] Error 1
make[1]: *** [Trilinos/packages/tpetra/core/src/CMakeFiles/tpetra.dir/all] Error 2
make: *** [all] Error 2
```
* It seems strange to me that the file `ccLluKcw.s` is located in /tmp/, when my build script build directory is actually located in `${GRP_DIR}/exnihilo/builds/`. 
* On the evening of 11/30, I e-mailed yong about this issue, originally thinking I was putting the build directory somewhere in ${HOME}, because that is where I put it when I built the TPLs. Yong told me that I was probably running out of space in HOME, and i should try to build in /global/scratch/${USER}/. I did this and at the end of the day I had a build that reached 100%. 
  * Although the build reached 100%, the install did not appear complete. The exnihilo/install directory was never created, and when I went to the build directory only the Omnibus tests were created. 
  * I e-mailed Tara to talk about it tomorrow. The new errors that I'm getting I think are Savio-agnostic, and I need some help with it.  

***

### Entry: 2015/11/30

#### Notes on day's progress ####

* Built gcc 5.2 in $TPL_DIR/packages
* Symlinked packages to top directory, $TPL_DIR
* Added gcc52env.sh file to source the necessary environmental variables that one will need to use gcc 5.2. Anybody should be able to use this by executing `$ source gcc52env.sh` 
  * When I add the install packages for gcc 4.8, I will create a similar shell script.
 
Package downloads for savio:
```Shell
$ wget http://sourceforge.net/projects/math-atlas/files/Stable/3.10.2/atlas3.10.2.tar.bz2
$ wget http://www.netlib.org/lapack/lapack-3.6.0.tgz 
$ wget http://www.open-mpi.org/software/ompi/v1.10/downloads/openmpi-1.10.1.tar.gz
$ wget https://www.hdfgroup.org/ftp/HDF5/current/src/hdf5-1.8.16.tar.gz
$ wget https://wci.llnl.gov/content/assets/docs/simulation/computer-codes/silo/silo-4.10.2/silo-4.10.2-bsd.tar.gz
$ wget http://sourceforge.net/projects/pcre/files/pcre/8.37/pcre-8.37.tar.gz
$ wget http://sourceforge.net/projects/swig/files/swig/swig-3.0.7/swig-3.0.7.tar.gz
```

After this I unpacked each folder in `$TPL_DIR/src` and executed:

```Shell
$ module unload intel
$ module load qt
$ cd $EXNIHILO_DIR/src/Exnihilo/install
$ ./install.sh pcre
$ export LD_LIBRARY_PATH=/global/home/groups/ac_nuclear/TPLs/packages/pcre/lib:$LD_LIBRARY_PATH
$
$
$
$ ./install.sh swig
$
$
$
$ ./install.sh openmpi
$ export PATH=/global/home/groups/ac_nuclear/TPLs/packages/openmpi/bin:$PATH
$ export LD_LIBRARY_PATH=/global/home/groups/ac_nuclear/TPLs/packages/openmpi/lib:$LD_LIBRARY_PATH
$
$ ./install.sh hdf5
$ export PATH=/global/home/groups/ac_nuclear/TPLs/packages/hdf5/bin:$PATH
$ export LD_LIBRARY_PATH=/global/home/groups/ac_nuclear/TPLs/packages/hdf5/lib:$LD_LIBRARY_PATH
$
$
$ ./install.sh silo
$
$
$ ./install.sh lava
$
$
$ cd $TPL_DIR/src/xianyi-OpenBLAS-afedc8e
$ make
$ make PREFIX=/global/home/groups/ac_nuclear/TPLs/packages/OpenBLAS install

```
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXCXX UPDATE THIS ASAP XXXXXXXXXXXXXXXXXXXXXXXXXXX


***

### Entry: 2015/11/25

Today was a writing day. I focused on outlining a research statement and a teaching statement. 

***

### Entry: 2015/11/23

#### Updates on -fPIC ####

**I am caught in dependency hell** 

I suspected that because the -fPIC suggestion occurs with `/global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against '.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC`, I thought that the error might be a result of not gcc, but because hdf5 was not compiled with -fPIC. **I got confirmation with Seth that Everything you link against needs -fPIC: HDF5, OpenMPI, GCC, GMP, Silo, the works.**

I need to recompile all of these TPLs on Savio now. My plan:
* Modify bootstrap.sh to build all TPLs in one folder: `${GRP_DIR}/TPLs/`, so all of our TPLs can be sourced from the same location. 



***

### Entry: 2015/11/22

####gcc 5.2 notes and errors:####

TL;DR summary:
You can't use gcc 5.2 with the intel compilers.

* This past week I got together with Kelly and built gcc 5.2. We followed these directions:
  * http://en.librehat.com/blog/build-gcc-5-dot-2-on-rhel-6/
  * We changed two steps:
    * First, before the configure: `$ module unload intel` to ensure that the new gcc was compiled with the existing gcc module on savio
    * Second, two lines were changed in the configure script: 
    
      ```Shell
      ../configure --prefix=$HOME/gcc5   ---> to ../configure --prefix=${GRP_DIR}/gcc5
             --disable-multilib 
             --enable-languages=c,c++ 
             --enable-libstdcxx-threads 
             --enable-libstdcxx-time 
             --enable-shared 
             --enable-__cxa_atexit 
             --disable-libunwind-exceptions 
             --disable-libada 
             --host x86_64-redhat-linux-gnu 
             --build x86_64-redhat-linux-gnu 
             --with-default-libstdcxx-abi=gcc4-compatible  ---> deleted 
      ```
      
    * The last line was deleted because we intentionally wanted to rebuild the c++ libraries for gcc5.2, rather than using all of the old ones from 4.4 (which are outdated)
    * Note also that we used the same environment variables detailed in that webpage:
    
      ```Shell
      export GCC5_ROOT=$GRP_DIR/gcc5
      export PATH=$GCC5_ROOT/bin:$PATH
      export MANPATH=$GCC5_ROOT/share/man:MANPATH
      export INFOPATH=$GCC5_ROOT/share/info:$INFOPATH
      export LD_LIBRARY_PATH=$GCC5_ROOT/lib64:$GCC5_ROOT/lib:$LD_LIBRARY_PATH
      export LD_RUN_PATH=$GCC5_ROOT/lib64:$GCC5_ROOT/lib:$LD_RUN_PATH
      export LIBRARY_PATH=$GCC5_ROOT/lib64:$GCC5_ROOT/lib:$LIBRARY_PATH
      export INCLUDE_PATH=$GCC5_ROOT/include:$INCLUDE_PATH
      export CPLUS_INCLUDE_PATH=$GCC5_ROOT/include:$CPLUS_INCLUDE_PATH
      export C_INCLUDE_PATH=$GCC5_ROOT/include:$C_INCLUDE_PATH
      ```
      
* Using this version of gcc to compile derp.cc and derp_plus_plus.cc worked, but only if g++ and not icpc was used to compile the script. It appeared that the libraries could not link properly, so the errors looked almost the same as with using gcc 4.4. An example is:
 
      ```
      In file included from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/random(49),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/bits/stl_algo.h(66),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/algorithm(62),
                 from derp_plus_plus.cc(3):
/global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/bits/stl_iterator_base_types.h(154): error: class "std::vector<std::seed_seq::result_type={std::uint_least32_t={unsigned int}}, std::allocator<std::seed_seq::result_type={std::uint_least32_t={unsigned int}}>>" has no member "iterator_category"
        typedef typename _Iterator::iterator_category iterator_category;
                                    ^
      In file included from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/random(49),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/bits/stl_algo.h(66),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/algorithm(62),
                 from derp_plus_plus.cc(3):
          detected during:
      In file included from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/random(49),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/bits/stl_algo.h(66),
                 from /global/home/groups/co_nuclear/gcc5/bin/../include/c++/5.2.0/algorithm(62),
                 from derp_plus_plus.cc(3):
            instantiation of class "std::__iterator_traits<_Iterator, void> [with _Iterator=std::vector<std::seed_seq::result_type={std::uint_least32_t={unsigned int}}, std::allocator<std::seed_seq::result_type={std::uint_least32_t={unsigned int}}>>]" at line 163
      ```
  
  * I also rechecked the compilers, just in case they were loaded improperly:
  
      ```Shell
      $ which gcc
      /global/home/groups/ac_nuclear/gcc5/bin/gcc
      $ which icpc
      /global/software/sl-6.x86_64/modules/langs/intel/2015.0.090/bin/intel64/icpc 
      ```

  * Using this version of gcc to build Exnihilo failed without even getting to the build. The c++ flag checks failed.  
  * After doing some research, I found out that gcc 5.2 is too new for the intel compilers. Seth confirmed that *Intel 2015 requires gcc 4.1 to 4.9* The websites with documentation confirming this are:
    * https://software.intel.com/en-us/forums/intel-c-compiler/topic/556968
    * http://stackoverflow.com/questions/29371301/intel-c-compiler-what-is-highest-gcc-version-compatibility
    * https://software.intel.com/en-us/articles/intel-parallel-studio-xe-2015-composer-edition-c-release-notes
    * https://bitbucket.org/fenics-project/dolfin/issues/375/intel-compiler-does-not-accept-some-c-11
    
####gcc 4.8.2 notes and errors:####

TL;DR summary: I have no idea why gcc 4.8 is giving me these errors. 

* After realizing the gcc 5.2 incompatibility, I tried to build gcc 4.8.2. I used the packages listed at http://btorpey.github.io/blog/2015/01/02/building-clang/ but executed everything line by line, as I had with gcc 5.2. 
  * Trying to build exnihilo results in an error that says to rebuild with an -fPIC flag
  
      ```
      [ 18%] Building CXX object
      Trilinos/packages/triutils/src/CMakeFiles/triutils.dir/Trilinos_Util_read_hb.cpp.o
      ld:
      /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against `.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC
      /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a: could not read symbols: Bad value
      make[2]: *** [Exnihilo/packages/Nemesis/libNemesis.so.6.2] Error 1
      make[1]: *** [Exnihilo/packages/Nemesis/CMakeFiles/Nemesis.dir/all] Error 2
      make[1]: *** Waiting for unfinished jobs....
      ```
      
  * The following page details a few different ways that one might encounter an -fPIC flag: https://wiki.gentoo.org/wiki/Project:AMD64/Fixing_-fPIC_Errors_Guide

* After trying to build gcc 4.8.2 the same way I built 5.2, I talked to Seth. He pointed out that the exnihilo install scripts can also build gcc from source, with the -fPIC flag enabled when all of them are compiled. 
  * This requires that you have created a `bootstrap.sh` file (now included in  `/install/rc/savio`). Then you execute:
  
      ```Shell
      ./install.sh -t bootstrap gmp
      ./install.sh -t bootstrap mpfr
      ./install.sh -t bootstrap mpc
      ./install.sh -t bootstrap gcc
      ```
      
  * Unfortunately, trying to build Exnihilo even with gcc compiled this way leads to the same `-fPIC` error. 
  
* Other things I've checked:
  * Adding the following compiler flags to `base.sh`:
  
      ```Shell
      export CFLAGS="${CFLAGS} -fPIC"
      export CXXFLAGS="${CXXFLAGS} -fPIC"
      export FCFLAGS="-fPIC"
      export F90Flags=${FCFLAGS}
      ```
      
  * Setting up my environment variables differently:
    1. creating the following `env.sh` and executing `source env.sh` before compiling:
    
      ```Shell
      export GCC48_ROOT=$HOME/prefix/gcc
      export GCCROOT2=$HOME/prefix
      export PATH=$GCC48_ROOT/bin:$GCCROOT2:$PATH
      export MANPATH=$GCC48_ROOT/share/man:$MANPATH
      export INFOPATH=$GCC48_ROOT/share/info:$INFOPATH
      export LD_LIBRARY_PATH=$GCC48_ROOT/lib64:$GCC48_ROOT/lib:$LD_LIBRARY_PATH
      export LD_RUN_PATH=$GCC48/lib64:$GCC48/lib:$LD_RUN_PATH
      export LIBRARY_PATH=$GCC48_ROOT/lib64:$GCC48_ROOT/lib:$LIBRARY_PATH
      export INCLUDE_PATH=$GCC48_ROOT/include:$INCLUDE_PATH
      export CPLUS_INCLUDE_PATH=$GCC48_ROOT/include:$CPLUS_INCLUDE_PATH
      export C_INCLUDE_PATH=$GCC48_ROOT/include:$C_INCLUDE_PATH
      ```
      
    2. adding the environment variables that are defined in bootstrap.sh to my base.sh file
    
      ```Shell
      export PATH=/global/home/users/munkm/prefix/gcc/bin:${PATH}
      export prefix_base=${HOME}/prefix
      export ld_paths=${prefix_base}/gmp:${prefix_base}/mpfr:${prefix_base}/mpc
      export LD_LIBRARY_PATH=${ld_paths}:${LD_LIBRARY_PATH}
      ```
      
    3. only adding the following to my `PATH` and `LD_LIBRARY_PATH`, which Yong said were the minimum number of variables to add to ensure the new gcc can work together with the intel compilers. 

      ```Shell
      export PATH=$HOME/prefix/gcc48/bin:$PATH
      export LD_LIBRARY_PATH=$HOME/prefix/lib64:$LD_LIBRARY_PATH
      ```

    4. executing `module unload gcc` before setting either steps 1., 2., or 3. listed above. 
  * All of these methods yielded the same -fPIC error. 

* I think that because the error has to do with linking an HDF5 library, I am wondering if HDF5 also has to be built with the -fPIC flag. I've sent e-mails to both Seth and Yong about this. 
  * Yong suggested building Exnihilo without intel, and with gcc only. I don't think this will work though, because all of the other libraries we're using (HDF5, etc.) are compiled using the intel compilers. So this error might persist. 
  * I'm waiting on replies from both Seth and Yong before I pursue anything further. 
  
#### Disc Writing errors ####

TL;DR summary: Don't let /tmp/ fill up or you won't be able to build exnihilo. 

* The exnihilo build writes to /tmp/, which has a limit of ~3.9Gb. This fills up easily during the build, so if you're debugging a build (like I have been doing) you can get write errors. That looks a little bit like the following: 

      ```
      icpc: warning #10352: The directory '/tmp' is full.  Please check disk space.
      icpc: error #10001: could not find directory in which g++ resides
      
      ...

      No space left on device
      FATAL ERROR: fwrite on file failed
      ": internal error: ** segmentation violation signal raised **
      Access violation or stack overflow. Please contact Support.
      ```

* To check if /tmp/ is full, Aron suggested this command: `df -H | head -1; df -H | grep "/tmp"`


####Advantg notes:####

* Advantg is mirrored at ssh://angband/repos/mirror/advantg.git

* Advantg can access the python modules from Denovo. To do this:
  * Make sure that your python file is loaded. For example, my file:
    * `/Exnihilo/packages/Denovo/pykba/integrator.py` is added to the `CMakeLists.txt` file. 
      ```Shell
      IF (ENABLE_PYTHON_WRAPPERS)
      # Bare Python files used for unit testing, script execution
      SET(PYTHON_MODULES
        integrator.py
        kba_runner.py
        kba_unittest.py
        )
      ```


***

### Entry: 2017/02/09

To get timing data, run advantg with the debug log on. 

advantg --log=DEBUG problem.py

***

### Entry: 2017/02/16

Notes for the end of the day:
* noticed that only ueki problems ended up with source biasing. Need to make sure that is right. 
  * mcnp options detailed in sec 5.8.1 of advantg manual.
* considering using violin plots in seaborn for anisotropy quantification:
  * http://seaborn.pydata.org/examples/
* beam problem has energy cutoff. Results in adj problem having only 5 energy groups but fwd still has 27. Can make WWs with differently sized things. Need to build in an exception to account for this. 
* ueki problems have zero fluxes. need an exception error because divide by zeros end up w/ nasty shiz. 
* used ornl ganglia page to view run memory usage. 

To do list:
  * get timing data for problems that ran
  * write script to report varios FoM values
  * write script to extract tally results
  * write script to report which tally tests failed 
  * get a few visuals of problems with interesting anisotropy data
  * make script to plot distribution of WW bins 
  * make a script to plot FoM by energy group 
  * make a script to show FoM label over violin plot 

***

### Entry: 2017/02/17

The http push/pull stuff was acting weird on remus/romulus and giving me a 403 error. 
I got around this by editing the .ssh/config file *in the repository* to be 
`munkm@github.com/munkm/repo.git` rather than `https://github.com/munkm/repo.git`. 
That error was documented here:
  * http://sharadchhetri.com/2014/08/16/error-requested-url-returned-error-403-forbidden-accessing-github-repo/

***

### Entry: 2017/02/21

Weight window debugging log:
* On 02/19 I discovered that the WW target value outputted by the automated method did not match that of the manually outputted WWS in prob_2. 
  * The only difference between these inputs deterministically was the weight windows
Tasks:
  * See if manually generating WWs for new problem run generates the same target or if they do something different
  * scp data from remus to get maze problem onto local machine
  * compare the weight window exit values between automated version

Results:
  * ipython notebook for this purpose was made at: `~/Documents/ORNL/old-files/WWgenerator-sandbox.ipynb`
  * 

***

### Entry: 2017/02/22

Debugging WW issue:
  * checked out to commit `#38a7748`, the commit build hash for the original PHYSOR problems; made new branch `ahm-test`
  * ran the exact PHYSOR variant (except changed pn order from 2 to 3) on Remus
    * manually calculated the `w_norm` value from PHYSOR run. Resulted in same value in automated current script. 
    * data pulled from remus onto local machine. Located at `Documents/ww-debug/test`
  * pulled in PHYSOR data, manually calculated w_norm. Value still matches the data from PHYSOR. 
    * Data located at: `/Siberia/Physor_problems/maze2_test_vacuum/`
  * Tried using data from Remus run to calculate w_norm for jsut the cadis run. Results in a different number. Used `Inegrator(adj,adj)`, which should collapse to the same adj fluxes
    * This could be due to the quadrature weight issue that I talked about with Tom a while ago. 
  * checked to see if sb_generator class and mcnp output classes have changed significantly from that commit hash to now. 
    * nothing in `SBGenerator.cpp` or `.hpp` changed in the calculation of `w_norm`
    * `advantg/outputs/mcnp` has both `outputs.py` and `deck.py` changed. 
  * checked that no matter what type of source biasing, w_norm is the same. 
    * space, energy = same
    * space only = same
    * none = same

Next steps:
* make sure (adj,adj) outputs in the same exact matrix as the collapsed flux. 
* add print statements to new function to see if the data changes unexpectedly at all
* modify old version of integrator to read in new files or vice versa
* talk to seth tomorrow. 

***

### Entry: 2017/02/23

* Seth told me he pushed an update to fix pn order 2 to master. 
  * decided to rebase current branch w/ master and rerun to see if the same output reults. 
  * tried to check out `angular_hybrid_method`
```
$ git checkout angular_hybrid_method
Downloading environment/latex/NsedTitle.pdf (8.25 MB)
Error downloading object: environment/latex/NsedTitle.pdf (9eb869fb712e17efdd7d5a4c3ee257063475b2b46af86dd09845318692e0801c)

Errors logged to /Users/madicken/Software/Exnihilo/.git/lfs/objects/logs/20170223T130046.198839791.log
Use `git lfs logs last` to view the log.
error: external filter git-lfs smudge -- %f failed 2
error: external filter git-lfs smudge -- %f failed
fatal: environment/latex/NsedTitle.pdf: smudge filter lfs failed 
```   
  * tried commenting out lfs commands in `~/.gitconfig`
```
$ git checkout angular_hybrid_method
error: Your local changes to the following files would be overwritten by checkout:
    .gitattributes
    Version.cmake
    environment/README.rst
    environment/emacs/nemesis-functions.el
    environment/emacs/nemesis-modes.el
    environment/emacs/nemesis.el
    environment/emacs/omnibus-mode.el
    environment/etc/templates/CMakeLists.txt
    environment/etc/templates/template.cu
    environment/etc/templates/template.cuh
    environment/etc/templates/template.hh 
Please, commit your changes or stash them before you can switch branches.
error: The following untracked working tree files would be overwritten by checkout:
...
... 
 Please move or remove them before you can switch branches.
Aborting
```
  * tried putting git lfs commands back in .gitconfig. Same error as immediately above responds.
  * tried `git reflog` and resetting to the state before these errors arose. No change. (note that the changes were not with reset hard) 

  * tried uninstalling the port for git lfs, commenting out the lfs lines in .gitconfig, and deleting the lfs hook in .git/hooks/. 
    * grepped in /hooks/ to see which hooks pertained to git lfs. 
    ```$ grep -r 'lfs' .
./pre-push:command -v git-lfs >/dev/null 2>&1 || { echo >&2 "\nThis repository is configured for Git LFS but 'git-lfs' was not found on your path. If you no longer wish to use Git LFS, remove this hook by deleting .git/hooks/pre-push.\n"; exit 2; }
./pre-push:git lfs pre-push "$@" ```
     * deleted pre-push
     * perfomed a `git reset --hard ` to the commit before everything got messed up. 
     * tried checking out again:
```
$ git checkout angular_hybrid_method
error: The following untracked working tree files would be overwritten by checkout:
    README.md
    environment/etc/templates/template.c
    environment/etc/templates/template.gg.omn
    environment/etc/templates/template.h
    environment/etc/templates/template.i.cuh
    environment/etc/templates/template.raytrace.ipynb
    environment/etc/templates/template.technote.tex
    environment/git/format_commit_messages.py
    environment/git/post-receive
    environment/git/pre-receive
    environment/git/pre-receive-denovo
    environment/git/pre-receive-locked
    environment/git/pre-receive-reasonable.sh
Please move or remove them before you can switch branches.
Aborting
```
* after doing the delete lfs, and `reset hard`, seth told me to do `git clean -fd`
```
madmunk@Rasputin:~/Software/Exnihilo(ahm-test) $ git status
On branch ahm-test
nothing to commit, working directory clean
madmunk@Rasputin:~/Software/Exnihilo(ahm-test) $ git checkout angular_hybrid_method
Switched to branch 'angular_hybrid_method'
Your branch is ahead of 'origin/angular_hybrid_method' by 2 commits.
  (use "git push" to publish your local commits)
madmunk@Rasputin:~/Software/Exnihilo(angular_hybrid_method) $ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```
  HURRAYYYYYYYYY

***

### Entry: 2017/02/24

* I checked the output of the pn order 2 that ran last night. Cadis->Cadis match 
between the different versions, but manualcadis-->cadisangle do not. They are off by 
about 4 orders of magnitude.
  * I submitted a run without the quantify_anisotropy function on. This should tell 
    me if the quantification is somehow overwriting the angle methods. 
    * Running cadisangle with quantify_anisotropy on and off result in the same w_norm. 
    * w_norm for these is 1.416035067e-09, whereas with the old script it is 
      1.15962549904e-06
* Yesterday I discovered with the new script that (adj,adj) does not return the same value as CADIS for w_norm. 
  * Tried today with the old script. 
    * actual cadis (oldfileformat, old build, pn2) = `2.672032757e-05`
    * old script (adj,adj) = `7.56906677871e-05`
    * ratio = 2.832
    * actual cadis (newfileformat, new build, pn3 (not pn2 compatible)) = `2.676207683e-05`
    * new script (adj,adj) = `7.61486845621e-05`
    * rato = 2.845 
    * actual cadis (newfileformat, new build, pn2) = `2.672032757e-05`
    * new script (adj,adj) = `7.56906677871e-05`
  * Note: I made a version of the integrating function that was backwards compatible with the old .h5 fileformat. 
* Okay, so I have found that the integrator script appears to do the same thing when manually generating the wnorm values. 
  * Why is it for pn 3 the number for wnorm is so different?
  * Why did the automated script generate a different w_norm than the manually generated one for pn order 2?
* Sanity check: ensure that for the *same data* on a full pn2 calculation the angle method generates the same wnorm automatically and manually. 
  * manually w_norm = 1.15962549904e-06
  * automatically w_norm = 1.416035067e-09
  * differences here are that manual was calculated with new input made BC with old 
    .h5 file format, automatic was with new integrator w/o old .h5 compatibility.
Notes:
* could it be that using the quadrature weights from the forward problem is wrong? 
  The weights should match since the problem is rotationally symmetric. 
* could it be the way I'm loading data into fieldMpDouble?

* Sanity check 2: see that the lavafile doesn't make a difference. 
  * w_norm is the same using lavafiles from different runs.
  * This is expected because the lavafile is outp, generated by the mcnp run. Even though pn order differs between these two runs, the outp file should be identical.
* Sanity check 3: see that the type of FieldMp doesn't make a difference. 
  * changing FieldMpDbl from `field=FieldMpDouble(mesh,n_groups,p_groups, 0,26)` 
    to `field=FieldMpDouble(mesh,n_groups,p_groups, NEUTRON, 0, NEUTRON, 26) ` 
    did not affect w_norm

Notes:
* Could it be the group bounds? I am loading things in differently than what this thing is expecting, probably. 
  * Modify code to be the same as the /denovo/solver.py in terms of checks for group bounds
  * Try hardcoding in the number of groups and see if it changes the number. 
  * could it be the mesh? 
* built modified version of advantg on node 22 of remus. Rebuild there. 

Tasks from two days ago:
* **make sure (adj,adj) outputs in the same exact matrix as the collapsed flux.**

* **add print statements to new function to see if the data changes unexpectedly at all**

* [DONE] **modify old version of integrator to read in new files or vice versa**

* [DONE] **talk to seth tomorrow.** 

To do list:
* Build in python assertions everywhere in advantg and in exnihilo. 

***

### Entry: 2017/02/25

* discussion with trisha: consider if angular flux data is not centered around the average (max-skewed or min-skewed), neither max/min or max/average capture that well. The ratio between the two values might be valuable in this case. 
* also consider mean vs. median
* quantifying variance? 


***

### Entry: 2017/02/27

* try 1: in `/Users/madicken/Documents/WW_debug/test/` read in adj
  * (adj, adj) using new script, pn2 compatible = 7.61486845621e-05
    * note: this is branch angular_hybrid_method w/o BC on old hdf5 files
  * standard cadis = 2.676207683e-05
    * in `~/munk_analysis/demonstration/cadisangle2/testcadis/output`
* try 2: in same folder, read in adj and fwd
  * (fwd, adj) using new script, = 7.185880146e-10
    * note: this is branch angular_hybrid_method w/o BC on old hdf5 files
  * automated method = 7.18588014571e-10
    * in `/munk_analysis/demonstration/cadisangle2/testanglecadis/output`
* try 3: in same folder, read in adj and fwd
  * (adj, fwd) using new script = 6.61553483984e-11
    * note: this is branch angular_hybrid_method w/o BC on old hdf5 files
* try 4: in `/Volumes/Siberia/Gulag/PHYSOR_problems/maze2_test_vacuum/`
  * (fwd, adj) pn order 2 using old script = 1.15962549904e-06
    * note: this script is not fwd compatible with new hdf5 files
  * automated method on pn order 2 = 1.416035067e-09 
    * data for this is located at: `~/munk_analysis/demonstration/wwdebug/PHYSOR/testangle/output`
* try 5: in `/Volumes/Siberia/Gulag/PHYSOR_problems/maze2_test_vacuum/`
  * (fwd, adj) pn order 2 using new script with old ff BC = 1.15962549904e-06 
    * note this is the updated integrator but without forwards compatability w/ new format
* try 6: run pn order 2 on local machine over again
  * automated result: 1.416035067e-09
* try 7: read in data from pn order 2 with updated script
  * (fwd, adj) pn order 2 using new script 1.41603506711e-09 


* try 1-3 was data from pn order 3 runs, try 4-7 is pn order 2 runs.
  * 4-5 are old pn order 2 runs run at Exnihilo commit #38a77488, advantg commit #3ad1078a
  * 6-7 are new pn order 2 runs run at Exnihilo commit #91fa625e, advantg commit #d593cb0d

* moved `~/Documents/WW_debug/` to `~/Volumes/Siberia/Gulag/WW-debug/` because the files were big. 

* Observations: because the automated and manual match for the same dataset, something has happened with the output of the hdf5 files. need to compare pn order 2 at `/Volumes/Siberia/Gulag/PHYSOR_problems/maze2_test_vacuum/` to `/Volumes/Siberia/Gulag/WW-debug/pn2`
   * diff the two files:
```
$ diff maze2.py  ~/Documents/pn2/maze2.py
4c4
<     "method":                    "cadis",
---
>     "method":                 "cadisangle",
15,18c15,17
<     "denovo_hdf5_angles":        True,
< #     "denovo_x_blocks":           2,
< #     "denovo_y_blocks":           2,
< #     "denovo_z_blocks":           3,
---
>     "denovo_x_blocks":             2,
>     "denovo_y_blocks":             2,
>     "denovo_z_blocks":             2,
```

To do list:
* compare forward and adjoint hdf5 files between old and new versions
  * old: /Volumes/Siberia/Gulag/PHYSOR_problems/maze2_test_vacuum/
  * new: ~/Documents/pn2/ 
* check to see if solvers have same defaults
* check to see if adjoint matrix is now rotated maybe?

From patricia talk:
* taking difference of mean-median will result different values depending on range
  * take difference as a percentage of the range

***

### Entry: 2017/02/28

* Made a notebok in `~/Documents/ORNL/old_files/HDF5-comparison.ipynb`
* load in old and new fwd and adj datasets
* Compared the files from then to now:
  * compared angles, mesh_g, _y, _x, and quadrature_weights 
    * fwd and fwd are identical
    * adj and adj are identical
    * This leads me to believe that the **structure** of each matrix has remained the same between then and now. 
  * compared the angular flux values between then and now
    * fwd and fwd
      * [1,1,1,1] difference between the two of them on the order of 10e-08 to 10e-12
      * found the mean value of each matrix, subtracted the difference:
        ```
            A=new_angular_flux.mean()
            B=old_angular_flux.mean()
            print(A, B, A-B)
        ``` 
        result:
        ```
        (0.00033741054301550025, 3.8351647233030862e-08, 0.00033737219136826722)
        ```
    * adj and adj
      * [1,1,1,1] difference between the two of them on the order of 10e-28
      * found the mean value of each matrix, subtracted the difference:
        ```
            A=new_angular_flux.mean()
            B=old_angular_flux.mean()
            print(A, B, A-B)
        ``` 
        result:
        ```
        (0.00032693007657824815, 0.0003269300765782481, 5.4210108624275222e-20)
        ```
     * **This leads me to believe that the fowrard problem that was solved is significantly different between then and now. Let us try to figure out why.** 
  * FUTURE NOTE: Put histograms of fwd distributions that you generated here. 

To do when getting home:
  * compare fields.silo from old vs. new data
  * compare mcnp inputs from old vs. new data
  * check that fields.silo and denovo_output.silo in old matches (only read in one energy group to be sure.
  * check that fields.silo and denovo_outputs.silo in new matches (only read in one energy group to be sure. 
  * die. 


