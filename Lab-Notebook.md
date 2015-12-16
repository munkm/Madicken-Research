### Entry List
### Entry Links: ###

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

### Entry: 2015/12/15

#### Running Tests: ####

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