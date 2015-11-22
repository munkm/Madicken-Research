### Entry List
* [Entry: 2015/11/22](#entry-20151122)

***
### Entry: 2015/11/22

**gcc notes:**

* Much of my week has been going back and forth with Yong Quin and Aron Roberts on module support in Savio, and with Seth and Tara on stuff with Exnihilo. 
  * Right now I'm stuck on a compile issue that has to do with an -fPIC flag. My build fails linking some hdf5 file with the following error: 
    '''
    ld: /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against `.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC
    /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a: could not read symbols: Bad value
    '''
  * I'm also having issues debugging the build because /tmp/ keeps filling up. Aron and Yong are aware of this issue. 

**advantg notes:**

* I would like to finally get Exnihilo to build.  

