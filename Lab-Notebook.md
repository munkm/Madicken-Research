### Entry List
* [Entry: 2015/11/22](#entry-20151122)
* [Entry: 2015/11/23](#entry-20151123)

***

### Entry: 2015/11/22

#### Updates on -fPIC ####

I suspected that because the -fPIC suggestion occurrs with `/global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against '.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC`, I thought that the error might be a result of not gcc, but because hdf5 was not compiled with -fPIC. ** I got confirmation with Seth that Everything you link against needs -fPIC: HDF5, OpenMPI, GCC, GMP, Silo, the works. **

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

* 

