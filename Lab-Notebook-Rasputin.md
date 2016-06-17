### Entry List
### Entry Links: ###

* [Entry: 2016/06/14](#entry-20160614)
* [Entry: 2016/06/17](#entry-20160617)


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

### Entry: 2016/06/17

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
advantg built with no problems, but it failed one input test thanks to my modifications so far. 

***
