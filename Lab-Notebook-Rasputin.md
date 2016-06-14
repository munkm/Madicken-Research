### Entry List
### Entry Links: ###

* [Entry: 2016/06/14](#entry-20160614)


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


***
