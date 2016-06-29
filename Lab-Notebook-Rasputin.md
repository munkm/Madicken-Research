### Entry List
### Entry Links: ###

* [Entry: 2016/06/14](#entry-20160614)
* [Entry: 2016/06/16](#entry-20160616)
* [Entry: 2016/06/17](#entry-20160617)
* [Entry: 2016/06/21](#entry-20160621)
* [Entry: 2016/06/22](#entry-20160622)
* [Entry: 2016/06/28](#entry-20160628)

***

### Entry: 2016/06/14

Attempt #1:

Set macports python 2.7 to default with
```
```
Ran tests, 17/960 tests failed
```
The following tests FAILED:
	146 - Robus_tstCE_MPI_1 (Failed)
	853 - ShiftMC_transport_tstKCode_Solver_MPI_1 (Failed)
	855 - ShiftMC_transport_tstKCode_Solver_MPI_4 (Failed)
	867 - OmnibusFrontend_test_parser_commands_MPI_1 (Failed)
	872 - OmnibusFrontend_test_parser_validator_MPI_1 (Failed)
	885 - OmnibusFrontend_test_postprocess_celltally_MPI_1 (Failed)
	886 - OmnibusFrontend_test_postprocess_cyltally_MPI_1 (Failed)
	888 - OmnibusFrontend_test_postprocess_meshtally_MPI_1 (Failed)
	889 - OmnibusFrontend_test_postprocess_tally_MPI_1 (Failed)
	894 - OmnibusFrontend_regress_atlas_MPI_1 (Failed)
	902 - OmnibusFrontend_regress_kcode_smg_MPI_1 (Failed)
	948 - OmnibusDriver_tst_kcode_sce_MPI_1 (Failed)
	949 - OmnibusDriver_tst_kcode_sce_MPI_2 (Failed)
	950 - OmnibusDriver_tst_kcode_sce_MPI_4 (Failed)
	951 - OmnibusDriver_tst_kcode_sce_kde_MPI_1 (Failed)
	952 - OmnibusDriver_tst_kcode_sce_kde_MPI_2 (Failed)
	953 - OmnibusDriver_tst_kcode_sce_kde_MPI_4 (Failed)
```

Attempt #2:

The last attempt was made without rebuilding Denovo with the new python. This shouldn't 
affect anything, but what the heck.

Rebuilt Exnihilo
Ran tests
```
The following tests FAILED:
	146 - Robus_tstCE_MPI_1 (Failed)
	606 - DenovoKBA_solvers_tstArnoldi_MPI_2 (Failed)
	853 - ShiftMC_transport_tstKCode_Solver_MPI_1 (Failed)
	855 - ShiftMC_transport_tstKCode_Solver_MPI_4 (Failed)
	867 - OmnibusFrontend_test_parser_commands_MPI_1 (Failed)
	872 - OmnibusFrontend_test_parser_validator_MPI_1 (Failed)
	885 - OmnibusFrontend_test_postprocess_celltally_MPI_1 (Failed)
	886 - OmnibusFrontend_test_postprocess_cyltally_MPI_1 (Failed)
	888 - OmnibusFrontend_test_postprocess_meshtally_MPI_1 (Failed)
	889 - OmnibusFrontend_test_postprocess_tally_MPI_1 (Failed)
	894 - OmnibusFrontend_regress_atlas_MPI_1 (Failed)
	902 - OmnibusFrontend_regress_kcode_smg_MPI_1 (Failed)
	948 - OmnibusDriver_tst_kcode_sce_MPI_1 (Failed)
	949 - OmnibusDriver_tst_kcode_sce_MPI_2 (Failed)
	950 - OmnibusDriver_tst_kcode_sce_MPI_4 (Failed)
	951 - OmnibusDriver_tst_kcode_sce_kde_MPI_1 (Failed)
	952 - OmnibusDriver_tst_kcode_sce_kde_MPI_2 (Failed)
	953 - OmnibusDriver_tst_kcode_sce_kde_MPI_4 (Failed)
```

I have no idea why test 606 failed in this run when it hadn't in the past. Just to check i ran
`ctest -V -R DenovoKBA_solvers_tstArnoldi_MPI_2`. The test then passed. Now I'm really confused. 

However, the Omnibus tests I also had trouble with on Savio. On Savio when these failed (with 
the same error, I might add) `In /Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Shift/mc_transport/test/ShiftMC_transport_tstKCode_Solver.exe, overall test result: FAILED
1/1 Test #853: ShiftMC_transport_tstKCode_Solver_MPI_1 ...***Failed  Error regular expression found in output. Regex=[overall test result: FAILED] 20.73 sec`
the reason was because MPI was having issues. 

Going into the cmake log, I noticed that the openmpi being used was located in `~/Install/`, not
`/opt/local/`, meaning I was sourcing an mpi I built myself, and not one built with macports.

Attempt #3

* Explicitly set openmpi to macports build version
* Comment out old mpi paths in base.sh Rasputin install script
* Delete Build dir
* Observation: old error in Trilinos build now not showing up
     ```
     warning: unknown warning option '-Wno-unused-local-typedefs' [-Wunknown-warning-option]
     warning: unknown warning option '-Wno-virtual-move-assign'; did you mean '-Wno-literal-conversion'?
      [-Wunknown-warning-option]
     ``` 
* Other observation: Lapack might not be found? 
     ```
     -- Performing Test HAVE_GCC_ABI_DEMANGLE
-- Performing Test HAVE_GCC_ABI_DEMANGLE - Success
-- Performing Test HAVE_TEUCHOS_BLASFLOAT
-- Performing Test HAVE_TEUCHOS_BLASFLOAT - Failed
-- Performing Test HAVE_TEUCHOS_BLASFLOAT_APPLE_VECLIB_BUGFIX
-- Performing Test HAVE_TEUCHOS_BLASFLOAT_APPLE_VECLIB_BUGFIX - Failed
-- Performing Test HAVE_TEUCHOS_BLASFLOAT_DOUBLE_RETURN
-- Performing Test HAVE_TEUCHOS_BLASFLOAT_DOUBLE_RETURN - Success
-- Performing Test LAPACK_SLAPY2_WORKS
-- Performing Test LAPACK_SLAPY2_WORKS - Failed
-- Performing Test LAPACK_SLAPY2_WITH_DOUBLE_WORKS
-- Performing Test LAPACK_SLAPY2_WITH_DOUBLE_WORKS - Success
-- Performing Test HAVE_TEUCHOS_LAPACKLARND
-- Performing Test HAVE_TEUCHOS_LAPACKLARND - Failed
     ```

Attempt #4:

* Rebuilt openmpi with llvm. 
  * Checked the openmpi cmake files in /Builds/openmpi/ and verified that I was using llvm distributed with my mac
  * Changed the following lines in base.sh:
  ```
  export CC=/usr/bin/llvm-gcc
  export CXX=/usr/bin/llvm-g++
  export F90=/opt/local/bin/gfortran
  
  export CFLAGS="-fPIC"
  ```
    from the original lines:
  ```
  export CC=/opt/local/bin/gcc-mp-4.9
  export CXX=/opt/local/bin/g++-mp-4.9
  export F90=/opt/local/bin/gfortran-mp-4.9
  ```
    then executed `./install.sh openmpi` and `./install.sh Exnihilo serial-debug`
* Built again

***

### Entry: 2016/06/16

Ok, I tried several variants with using the macports toolchain and a clang build of the toolchain.
I'm not going to include them all here because the errors are similar to what I've documented
in previous builds. But I e-mailed seth. 

Building with an entirely macports toolchain gives me a "virtual thunk" in Trilinos. 
Seth's comment is: 
> OK, build variant 1 is something we've seen a number of times but doesn't seem to get 
> fixed in Trilinos: the dreaded "virtual thunk". I thought it was only an issue when 
> explicit template instantiation (ETI) was enabled, but in yours it's not, so that's 
> confusing.

He also notes that in a macport build, hdf5 should be built with a "+gccXX flag, including hdf5 and openmpi (and silo too maybe?)"

What this looks like in macports is that the hdf5 port has a +gccXX appended to the name. 
For my build in macports the port name is `hdf5 @1.10.0_0+cxx+gcc49+hl`. 

Seth's two main build recommendations:
> 1. Macports everything, using the +gccXX flag, including hdf5 and openmpi (and silo too maybe?).
> 2. Clang compiler, with your own compiled versions of openmpi, hdf5, and silo that use 
> clang. (This is what I personally do.) You do have to have gcc installed somewhere to 
> provide the fortran compiler.

Unfortunately, there's not really a solution for the "virtual thunk", so recommendation #1 
is not really option. Seth's other recommendation is the way to go. 

My earlier builds used clang to build openmpi, but I hadn't rebuild hdf5 or silo, and my base.sh 
file used the macports gcc to build Exnihilo. I tried rebuilding hdf5, but some strange errors
appeared, and silo wouldn't build at all. I've done a few different calls to llvm to see if
it would change anything.

Some of my variants:

```
export F90=/opt/local/bin/gfortran
export CC=/opt/local/bin/gcc-mp-4.9
export CXX=/opt/local/bin/g++-mp-4.9
```

```
llvm-3.5
export CC=/opt/local/bin/llvm
export CXX=/opt/local/bin/llvm
export F90=/opt/local/bin/gfortran
```

```
export CC=/usr/bin/llvm-gcc
export CXX=/usr/bin/llvm-g++
export F90=/opt/local/bin/gfortran
```

I have a call with Seth and Tara tomorrow so I'll try to see where the build is failing 
in hdf5 with each of these.


***

### Entry: 2016/06/17

Call with Seth and Tara

It seems like I was very close to a successful build over the past few days. My hdf5 build
was failing due to C++ library stuff. The build errors with HDF5 looked like this:

```
[ 98%] Building CXX object c++/test/CMakeFiles/cpp_testhdf5.dir/h5cpputil.cpp.o
[ 98%] Linking CXX executable ../../bin/cpp_testhdf5
Undefined symbols for architecture x86_64:
  "H5::H5Location::dereference(H5::H5Location const&, void const*, H5R_type_t, H5::PropList const&)", referenced from:
      _test_reference in trefer.cpp.o
  "H5::Group::Group(H5::H5Location const&, void const*, H5R_type_t, H5::PropList const&)", referenced from:
      _test_reference in trefer.cpp.o
  "H5::DataSet::DataSet(H5::H5Location const&, void const*, H5R_type_t, H5::PropList const&)", referenced from:
      _test_reference in trefer.cpp.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make[2]: *** [bin/cpp_testhdf5] Error 1
make[1]: *** [c++/test/CMakeFiles/cpp_testhdf5.dir/all] Error 2
make: *** [all] Error 2
```

To remedy this, we changed `/codes/hdf5/base.cmake` from 
```
SET(HDF5_BUILD_CPP_LIB        ON             CACHE BOOL   "")
```
to
```
SET(HDF5_BUILD_CPP_LIB        OFF             CACHE BOOL   "")
```

This resulted in a successful clang build of hdf5, and of silo (which was failing yesterday
due to the failed hdf5 build. 

Exnihilo built with no problems
advantg built with no problems, but it failed one input test thanks  (I think) to my 
modifications for my method so far. 


***

### Entry: 2016/06/21

Over the past day (and a half) I've been struggling with advantg some more. I got successful 
builds on 
Rasputin for hdf5, silo, openmpi, advantg, and Exnihilo. I also ran all of the unit tests,
so I thought the builds were good. Yesterday when I was trying to determine the origin of 
the error I had trying to run the example problems (jpdr, to be specific), I ended up 
getting this output:

```
$ python jpdr.py
Processing user input...
INFO: Loading user input from 13 dictionary entries
            ...finished processing user input
INFO: Moved prior working directory 'output' to 'output_old_004'
Traceback (most recent call last):
  File "jpdr.py", line 39, in <module>
    run(inp)
  File "/Users/madicken/Install/advantg/lib/python2.7/site-packages/advantg/driver.py", line 236, in run
    return run_from_ui(read_input(userdict, filename))
  File "/Users/madicken/Install/advantg/lib/python2.7/site-packages/advantg/driver.py", line 151, in run_from_ui
    model = McnpModel.from_ui(model_opts, working_dir)
  File "/Users/madicken/Install/advantg/lib/python2.7/site-packages/advantg/models/mcnp/model_disabled.py", line 6, in from_ui
    raise RuntimeError("MCNP support is disabled in this build because "
RuntimeError: MCNP support is disabled in this build because Lava is not installed.
```

This is a bit odd, because (1) lava is explicitly turned off in my Exnihilo serial-debug.cmake
file. I never rebuilt lava on Friday, so I could be having an issue with my old build. 

I rebuild lava with the clang compilers and then tried to build Exnihilo. The exnihilo
build died at ~60%. The build failure looked like this:
```
[ 63%] Building CXX object Exnihilo/packages/Geometria/lava/CMakeFiles/Geometria_lava.dir/Lava_Source_Attr.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/shapes/Prism_Shape.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/lava/CMakeFiles/Geometria_lava.dir/Lava_Surface_Group.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/shapes/Sphere_Shape.cc.o
/Users/madicken/Software/Scale/Exnihilo/packages/Geometria/lava/Lava_Surface_Group.cc:171:9: error: use of undeclared identifier 'lava_ReInitSurface'; did you mean 'lava_GetSurfaceId'?
        lava_ReInitSurface(surf_idx + 1);
        ^~~~~~~~~~~~~~~~~~
        lava_GetSurfaceId
/Users/madicken/install/lava/include/lava.h:158:31: note: 'lava_GetSurfaceId' declared here
LAVA_API_FUNC(int)            lava_GetSurfaceId(int);
                              ^
1 error generated.
make[2]: *** [Exnihilo/packages/Geometria/lava/CMakeFiles/Geometria_lava.dir/Lava_Surface_Group.cc.o] Error 1
make[1]: *** [Exnihilo/packages/Geometria/lava/CMakeFiles/Geometria_lava.dir/all] Error 2
make[1]: *** Waiting for unfinished jobs....
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/Volumes.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/Utils.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/Universe.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/Universe_Completer.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/General_Universe.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/Builder.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/XML_Builder.cc.o
[ 63%] Building CXX object Exnihilo/packages/Geometria/gg/CMakeFiles/Geometria_gg.dir/GG_Geometry.cc.o
[ 63%] Linking CXX shared library libGeometria_gg.dylib
[ 63%] Built target Geometria_gg
make: *** [all] Error 2
```

I admit I wasn't watching the lava build properly, so I went back and looked at my lava build. 
The lava build completed, but I got a few warnings. In particular I got this

```
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: /Users/madicken/Install/lava/lib/liblava.a(parameters.f90.o) has no symbols
```
and this
```
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: /Users/madicken/Install/lava/lib/liblava.a(reinit.c.o) has no symbols
```
I searched around on the internet and it seems like this type of error can happen if [libraries
are compiled as static objects rather than shared](http://stackoverflow.com/questions/4929255/building-static-libraries-on-mac-using-cmake-and-gcc). 
I'm wondering if that will make a difference
but I don't want to rebuild all of the TPLS unless actually necessary.

I also double checked that the built liblava.a didn't have these objects in the file using
[the nm command](http://www.thegeekstuff.com/2012/03/linux-nm-command/). There are objects 
in the file, just not the two that I got warnings about. 

I asked seth a few related questions about this:
* Do I actually need lava for Exnihilo? Previous builds with lava had it turned off, not on. 
* Should I reclone lava?
* Should I rebuild all of the TPLs as shared objects or as static?
* Does pykba really need everything to be shared, or is -FPIC sufficient? 

***

### Entry: 2016/06/22

Lots of new stuff today:
* Lava should be disabled for Exnihilo, but can't be for advantg. I can turn lava OFF for
my Exnihilo build, but I need an install for advantg to have MCNP support. 
  * I rebuilt both Exnihilo and advantg with these specs
* Advantg had trouble finding lava in the install

```
-- Found MCNP: /Users/madicken/Install/mcnp/bin/mcnp5
-- MCNP version: mcnp     ver=5    , ld=10252010
-- Could NOT find Lava (missing:  LAVA_CMAKE_FILE LAVA_INCLUDE_DIR)
-- The Lava library was not found. The ADVANTG package cannot interpret MCNP without it. If Lava is installed on your system, add the Lava root directory to the CMAKE_PREFIX_PATH cache variable and re-run CMake.
-- Found HDF5: /opt/local/lib/libhdf5.dylib;/opt/local/lib/libz.dylib;/usr/lib/libdl.dylib;/usr/lib/libm.dylib (found version "1.10.0")
-- Found Silo: /Users/madicken/Install/silo/lib/libsiloh5.dylib
-- Found MPI_CXX: /Users/madicken/Install/openmpi/lib/libmpi_cxx.dylib;/Users/madicken/Install/openmpi/lib/libmpi.dylib
-- Found MPI_Fortran: /Users/madicken/Install/openmpi/lib/libmpi_usempif08.dylib;/Users/madicken/Install/openmpi/lib/libmpi_usempi_ignore_tkr.a;/Users/madicken/Install/openmpi/lib/libmpi_mpifh.dylib;/Users/madicken/Install/openmpi/lib/libmpi.dylib
-- Found SWIG: /opt/local/bin/swig (found version "3.0.9")
-- Found Doxygen: /opt/local/bin/doxygen (found version "1.8.10")
-- Configuring done
CMake Warning (dev) at test/mcnp/CMakeLists.txt:75 (add_dependencies):
  Policy CMP0046 is not set: Error on non-existent dependency in
  add_dependencies.  Run "cmake --help-policy CMP0046" for policy details.
  Use the cmake_policy command to set the policy and suppress this warning.

```

  	* Note that Lava was not found at all, and HDF5 is the macports build, not my custom 
  	build with clang. I'll need to define explicit paths to both lava and hdf5 to remedy 
  	this issue and build advantg correctly. 
  * I fixed this by making a Rasputin folder in `./codes/advantg/`, and adding a base.cmake
  file to that folder. base.cmake files are read by default for cmake builds. A variant file
  (like serial-debug.cmake) would require me to build advantg with the variant name 
  (e.g. `./install.sh advantg serial-debug` rather than `./install.sh advantg`. The 
  base.cmake file in that folder for my system reads as follows:
  
  ```
# HDF5 Paths
SET(HDF5_INCLUDE_DIR "/Users/madicken/Install/hdf5/include"    CACHE PATH "")
SET(HDF5_LIBRARY_DIR "/Users/madicken/Install/hdf5/lib" CACHE PATH "")

# LAVA Paths
SET(LAVA_LIBRARY_DIR "/Users/madicken/Install/lava/lib"     CACHE PATH "")
SET(LAVA_INCLUDE_DIR "/Users/madicken/Install/lava/include" CACHE PATH "")

INCLUDE(${CMAKE_CURRENT_LIST_DIR}/../base.cmake)

  ```
  
    * The last line `INCLUDE($...` sources the base.cmake file of the directory above my 
    system variant directory. This way I don't have to mess with the advantg default build. 
    If I wanted to ignore that I would just remove that last line.
    * To define the directories for HDF5 and lava I used `LAVA_INCLUDE_DIR` and whatnot. 
    This actually took some debugging. Originally I used `LAVA_INCLUDE_DIRS` (etc.) like
    magneto, orthanc, and remus. I also tried `TPL_LAVA_INCLUDE_DIRS` like sethjmacpro, but
    for whatever reason these definitions were not findable by cmake. The only really 
    relevant link documenting the difference between `DIRS` and `DIR` was [this cmake forum
    post](https://cmake.org/pipermail/cmake/2006-October/011502.html).
* I also asked Tara about building Exnihilo and the TPLS as shared libraries rather than static
  * She said it might make a difference, but the excerpt from yesterday (re: -FPIC and pykba
  built with shared libraries) reminded her to tell me pykba no longer exists in Denovo. 
  * Need to come up with action plan of where to put old integrator module. 
  
***

### Entry: 2016/06/28

* Today I pulled updated versions of Exnihilo, lava, advantg, TriBITS and Trilinos. 
  * I wasn't sure where skeleton-scale was located? I don't have it connected to a repo. 
* I coped install files from ahm_sandbox (my local branch) to rebuild
  * Didn't copy the variant copy of /codes/hdf5/base.cmake which had `CPP_LIB OFF`
* Rebuilt the software
  * Exnihilo
  * lava
  * advantg
* Builds completed for everything
* Ran test suites
  * Exnihilo
  
  ```
  99% tests passed, 8 tests failed out of 1042

Label Time Summary:
Denovo       = 146.31 sec (328 tests)
Geometria    =  16.33 sec (101 tests)
Nemesis      =  13.57 sec (154 tests)
Omnibus      =   6.12 sec (29 tests)
Physica      =   0.60 sec (12 tests)
Robus        =   1.80 sec (25 tests)
Shift        =  74.17 sec (129 tests)
Transcore    =  17.77 sec (144 tests)

Total Test time (real) = 715.52 sec

The following tests FAILED:
	  3 - Nemesis_tstVector_Lite_MPI_1 (Failed)
	479 - PhysicaSCE_tstComp_Modifier_MPI_1 (Failed)
	926 - ShiftMC_transport_tstKCode_Solver_MPI_1 (Failed)
	928 - ShiftMC_transport_tstKCode_Solver_MPI_4 (Failed)
	969 - OmnibusPython_test_parser_commands_MPI_1 (Failed)
	993 - OmnibusPython_test_postprocess_tally_MPI_1 (Failed)
	1003 - OmnibusPython_regress_denovo_MPI_1 (Failed)
	1005 - OmnibusPython_regress_kcode_MPI_1 (Failed)
Errors while running CTest
make: *** [test] Error 8
  ```
  
    * Some of these failures are concerning, like the Shift test failed because relative 
    errors exceeded what were expected. However, some of the tests failed like my earlier
    tests did -- because I don't have full Scale support. Here's a sample of a verbose
    output from test 1003:
    
    ```
    
1003: ======================================================================
1003: ERROR: test_regression (__main__.DenovoMlprecon)
1003: ----------------------------------------------------------------------
1003: Traceback (most recent call last):
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/scripts/omnibus_pre.py", line 21, in run
1003:     return OmniGenerator().run(inputs, outpath)
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/generator.py", line 212, in run
1003:     db.validate()
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/omn/root.py", line 192, in validate
1003:     Database.validate(self)
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/parser/database.py", line 444, in validate
1003:     function(stack)
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/parser/database.py", line 500, in _apply_commands
1003:     cmd.apply(value, self)
1003:   File "/Users/madicken/Builds/Exnihilo-serial-debug/Exnihilo/packages/Omnibus/python/omnibus/parser/commands.py", line 254, in apply
1003:     raise OmniRuntimeError(str(e), value)
1003: OmniRuntimeError: At /Users/madicken/Software/Scale/Exnihilo/packages/Omnibus/driver/test/input/denovo_mlprecon.omn:12: The SCALE data directory "" is invalid.:
1003:     test-8grp
1003:
1003: ----------------------------------------------------------------------
1003: Ran 7 tests in 0.267s
1003:
    ```
  
  * advantg
  
  ```
  95% tests passed, 3 tests failed out of 63

Total Test time (real) =   4.14 sec

The following tests FAILED:
	  7 - test_FuncSpectrum (Failed)
	 55 - py:test_models_sword (Failed)
	 61 - py:test_input (Failed)
Errors while running CTest
make[3]: *** [CMakeFiles/check] Error 8
make[2]: *** [CMakeFiles/check.dir/all] Error 2
make[1]: *** [CMakeFiles/check.dir
  ```
     
     * I'm trying to figure out the cause of these issues, but I'm not sure where they're coming from yet. TBD.
     
***