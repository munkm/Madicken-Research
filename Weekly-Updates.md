### Update List

* [Update: 2017/03/22](#update-20170322)
* [Update: 2016/07/27](#update-20160727)
* [Update: 2016/07/13](#update-20160713)
* [Update: 2016/06/29](#update-20160629)
* [Update: 2016/06/22](#update-20160622)
* [Update: 2016/06/15](#update-20160615)
* [Update: 2016/06/08](#update-20160608)
* [Update: 2016/06/01](#update-20160601)
* [Update: 2016/05/24](#update-20160524)
* [Update: 2016/05/24](#update-20160524)
* [Update: 2016/05/09](#update-20160509)
* [Update: 2016/04/19](#update-20160419)
* [Update: 2016/03/29](#update-20160329)
* [Update: 2016/03/07](#update-20160307)
* [Update: 2016/02/17](#update-20160217)
* [Update: 2016/02/17](#update-20160217)
* [Update: 2016/02/12](#update-20160212)
* [Update: 2016/02/05](#update-20160205)
* [Update: 2016/01/29](#update-20160129)
* [Update: 2015/12/15](#update-20151215)
* [Update: 2015/12/12](#update-20151212)
* [Update: 2015/12/05](#update-20151205)
* [Update: 2015/11/22](#update-20151122)
* [Update: 2015/07/29](#update-20150729)
* [Update: 2015/06/23](#update-20150623)
* [Update: 2015/01/26](#update-20150126)
* [Update: 2014/12/30](#update-20141230)
* [Update: 2014/12/22](#update-20141222)
* [Update: 2014/12/10](#update-20141210)
* [Update: 2014/12/03](#update-20141203)
* [Update: 2014/11/24](#update-20141124)
* [Update: 2014/10/29](#update-20141029)
* [Update: 2014/10/22](#update-20141022)
* [Update: 2014/10/15](#update-20141015)
* [Update: 2014/10/08](#update-20141008)
* [Update: 2014/10/01](#update-20141001)

***

### Update 2017/03/22

Since I'm starting over with these updates (I'll upload them around our meeting 
times on Wednesdays), I'm just going to say what I did over the past week.

* After reviewing the method from the basics, I convinced myself that we were missing 
  a factor of 4pi in the omega flux calculation.
  * I had a conversation with Steven about it. Steven talked to Tom and we agreed. 
* I took a break on rerunning analysis for a bit and went back to my analysis suite. 
  * Finished tools ready to analyze the anisotropies in a better way than before
    * Violin plots added
    * Strip Plots added
    * Better histogram plots
  * Finished an hdf5 reading tool to read in chunks of data, rather than the entire 
    anisotropy dataset at once.
    * load in all anisotropy types by a particular energy group
    * load in all energy groups for a particular anisotropy metric
  * Finished mcnp track length tally reader
    * pyne doesn't seem to have a tally reader for anything other than a meshtally (in the 
      future this will be useful, but not for the CADIS runs), I made a tool to read in the
      following data from a given output
      * tally bins, output and relative error
      * fom convergence by energy type
      * timing data from mcnp script 
  * Working on tool to automate the analysis of a single run. In the future I'd like this
    to be callable from a loop that goes over all of my run directories. 

This coming week:
* Modify omega- flux in denovo to have factor of 4pi
* Rebuild my software on local machine and on Remus
* Get python analysis suite on remus and callable from a script
* Document how to use analysis suite tools in notebooks (also in github repo), this can
  be more sophisticated later, but it'll be satisfactory for the meantime.
* Analysis tools:
  * tool to read in the timing data from denovo and output a dict of all four FoM types mentioned in
    analysis section of dissertation. 
  * finish tool to automate analysis of single run
* Go back to figuring out problems and why they're still not running quite right. 
  * Make a surface plot of therapy room wall thickness vs. mesh size (and surface plot color 
    corresponding to FoM)   


***

### Update 2016/07/27

Goals from last week:
* **Completed** Prepare for interviews 
* **Completed** Do a practice job talk w/ friends
* **Completed** Continue with rebuild w/Seth's e-mail advice
* **Started** Do a live build with Seth on Savio
* **Completed** Document savio installation information
* **Not Completed** Move method to new logical Denovo module (pending code review) 
* **Not Completed** Merge current versions of advantg and exnihilo into my dev branches
* **Not Completed** Remove python error from fwcadis and fwcadisangle

Goals for the upcoming week:
* Move method to new logical Denovo module (pending code review) 
* Merge current versions of advantg and exnihilo into my dev branches
* Remove python error from fwcadis and fwcadisangle
* Get working version of Exnihilo on Savio
* Get fully working version of Exnihilo on Rasputin
* Continue documenting build process on both machines
* Plan out writing schedule for the semester

Postponed goals: 

* Figure out why the python error is showing up with fwcadis method options 
but not fwcadisangle method options.

***

### Update 2016/07/13

Goals for the trip:

* **Not Completed** Work on writing some of my thesis 
  * Note: I did actually work on writing, but I didn't get even close to the amount I was 
  expecting to, so I don't consider this completed. To make up for the work I've missed, I plan 
  on working on the weekend this coming weekend. 
* **Attempted** Try and debug the rebuild, if possible. 

Goals for the upcoming week:

* Prepare for interviews 
* Do a practice job talk w/ friends
* Continue with rebuild w/Seth's e-mail advice
* Move method to new logical Denovo module (pending code review) 
* Merge current versions of advantg and exnihilo into my dev branches
* Remove python error from fwcadis and fwcadisangle

Postponed goals: 

* Figure out why the python error is showing up with fwcadis method options 
but not fwcadisangle method options.

***

### Update 2016/06/29

Goals from last week:
* [**Started Code review**]Move method to new logical Denovo module (pending code review) 
* [**Not Completed**]Merge current versions of advantg and exnihilo into my dev branches
* [**Not Completed**] Remove python error from fwcadis and fwcadisangle
* [**Not Completed**] Figure out why the python error is showing up with fwcadis method options 
but not fwcadisangle method options.
* [**Completed**]Prep for interviews

Goals for Work in India
* Work on writing up more of background of thesis
* Continue debugging the rebuild, if resources are available (I am not brining my RSA key, so
any new pulls won't be possible while I'm on the trip)

Postponed Goals 
* Move method to new logical Denovo module (pending code review) 
* Merge current versions of advantg and exnihilo into my dev branches
* Remove python error from fwcadis and fwcadisangle
* Figure out why the python error is showing up with fwcadis method options 
but not fwcadisangle method options.

***

### Update 2016/06/22

Goals from last week:

* **Completed** Rebuild Exnihilo with no errors (and rerun all tests)
* **Completed** Figure out why Exnihilo build errors are occuring
* **Completed** Rebuild advantg with updated TPLs and python
  * **Completed** See if carryover python errors from last week are related
* [**Carry Over**] Remove python error from fwcadis and fwcadisangle
* [**Carry Over**] Figure out why the python error is showing up with fwcadis method options 

General updates: 

* Friday I successfully built Exnihilo and advantg
  * One of the advantg unittests failed, but I believe it is due to my own changes
* Monday I discovered that advantg couldn't run the example problems due to lack of lava support
  * Lava build / debug process is documented in my [Rasputin Lab Notebook](./Lab-Notebook-Rasputin.md)
  * Spent some time getting lava support in Exnihilo (now not necessary)
  * Talked to Tara this morning and determined lava is NOT required for Exnihilo, but does
  need to be built for advantg. Remedied problem by making variant .cmake file for advantg 
  on Rasputin. 
* Python error occurs in both "moments" and my method branch, so this means that the 
error I was seeing a few weeks ago is related to incompatible Exnihilo and advantg stuff
  * I've updated the exnihilo and advantg master branches and am pending merging master into 
  my local branches (after I review and understand the code). 
  
* my method needs to be moved from pykba to a different module. 
* Reschedule meeting for 7/6 and 7/13
  * Seminar talk templates from Rachel? 
    * 2014 france talk 
    * 2015 davis talk
    * 2015 mines
    * 2015 UF
    * 2015 ORNL 
    * 2016 CoDA
    * 2015 INL

Goals for the next week:

* Move method to new logical Denovo module (pending code review) 
* Merge current versions of advantg and exnihilo into my dev branches
* [**Postponed**] Remove python error from fwcadis and fwcadisangle
* [**Postponed**] Figure out why the python error is showing up with fwcadis method options 
but not fwcadisangle method options.
* Prep for interviews

***

### Update 2016/06/15

Updates:

* On Monday we met and talked about last week's python errors. We decided to rebuild all of the TPLs and see if something in Exnihilo is related. 
  * Since then I've been working on the build
* Found that if I change fwcadisangle method options to new names, fwcadis runs and gets to the same error it fails on master. 
* fwcadis and fwcadisangle fail at different places, but both are related to the denovo solve option

Goals for the next week:

[**Carry Over**] * Remove python error from fwcadis and fwcadisangle
[**Carry Over**] * Figure out why the python error is showing up with fwcadis method options, but not fwcadisangle method options.
* Rebuild Exnihilo with no errors (and rerun all tests)
* Figure out why Exnihilo build errors are occuring
* Rebuild advantg with updated TPLs and python
  * See if carryover python errors from last week are related

***

### Update 2016/06/08

Updates:

* HDF5 printing in the Advantg _angle methods are recognized
  * Strange python from Denovo is appearing when I call them
  * JSON files with denovo_hdf5_angles = True exist for these methods
* When I try to run a normal fwcadis problem (no angle), the fwcadis method
  options are not recognized. This must have something to do with inheritance. 
  * Each method is imported as import .methods.cadis as CadisMethod, import 
    .methods.fwcadis as FwCadisMethod, so the method options should not be overlapping

Goals for the next week:

* Figure out why the python error is showing up with fwcadis method options, but not fwcadisangle method options.
* Remove python error from fwcadis and fwcadisangle

***

### Update 2016/06/01

Updates:
* Pushed the PHYSOR paper updates to [website](http://munkm.github.io/posts/2016/06/01/publications.html) and [paper repo](https://github.com/munkm/Papers/tree/master/PHYSORPaper_2016/paper). Also kept old copy and poster on my personal page. 
* Worked on method (this can be discussed in detail during our meeting)
* applied to postdocs
  * asked about start date flexibility.
* Submitted physor reimbursement. Couldn't find the receipt for the poster, but I'm hoping that will be all right.
* Didn't lose too much work after crash last week; all but 3 days were backed up (I didn't bring my external drive on my roadtrip with my parents).  

Goals for the upcoming week:
* Focus all time on implementation. I would really like to get an automated version running soon.
* Fix computer so all dev tools are up to date (I lost most of these during last week's crash

### Update 2016/05/24

Updates: 
* Not much since last time. Commencement and other stuff have prevented me from doing much research

Goals for the upcoming week:
* pushing updates to the PHYSOR paper (I didn't get a chance to do that at PHYSOR itself)
* working on the FW-CADIS automation in ADVANTG
* following-up on my postdoc interviews (my mic died for the BIDS interview. sigh.)
* applying for the terrapower (job posting) and IIASA (job posting) positions
* submitting PHYSOR reimbursement


### Update 2016/05/09

Updates:
* Went to PHYSOR. Made a lot of good connections.

Goals for the next few days.
* Apply to ORNL postdoc positions
* Interview for other postdocs
* Working on implementation

### Update 2016/04/19

Updates:
* Sent TPL wish list to Kelly / Savio
* I will start working on the Savio install again later next week. 
 
* Working on PHYSOR poster.
  * General stuff: 
    * Link [here](http://physor2016.org/includes/Speaker_PosterInstructions.pdf) for poster requirements
    * Tall?
    * Berkeley branding colors available at [berkeley branding webpage](http://www.berkeley.edu/brand/img/colors/WebColor_accessible_AA_swatches.pdf)
    * Finish by Monday, 4/25 so it can be printed. 
      * Printing service recommendations? I can front the cost since I'm submitting a reimbursement for the conference hotel anyways. 
      * Poster tube availability? Who should I talk to? 
  * Content:
    * Including information actually in the paper
    * Including results from group meeting 
    * Advice: which way is best to present FOMs for these parametric studies? 
      * Plot FOM/number of particles for different Cadis-Omega problems (multiple lines)?
      * Plot the FOM **in each energy bin** vs. number of particles for one problem as a colorplot (one plot for each case)? 
* Applications
  * Talk about BIDS/ other places I'm applying so Rachel is expecting e-mails from these places. 

* PHYSOR logistics
  * Handout re: april 8 e-mail

* Method work: still in progress. 

### Update 2016/03/29
Last Week:

* Still working on Savio install (sigh)
  * Found two different versions of Lapack; not sure why some functions not in Lapack folder's version of liblapack.a and in Atlas' folder of liblacpack.a
  * Not sure which Blas to link against with Atlas lapack lib
  * Taking a break from Savio Installs
  
* Job Applications
  * BIDS postdoc, IIASA fellowship, Nat'l Labs
  * Writing statements and letters
  * Updating github page to include more info. 
  * Still need to complete SW Carpentry instructor training

* PHYSOR
  * Registered through Sara
  * Reserved hotel room (has to be with my own money) 5/1-5/5
  * Still need to do car/plane

* Hybrid Method
  * Added functions to Advantg driver function. 
  * Still need to have MCNP output modification
    * Right now I have both the old MCNP output and the new one written to output.
    * This means timings will be irrelevant if we include writing times in runtime
    * Luckily Advantg outputs both the Denovo transport time AND the writing times 

### Update 2016/03/07

Past while:
* I worked on finishing up the tool to quantify anisotropy
  * We can use that tool to plot data for the talk Rachel is giving in a few weeks. 
* Testing phase space
  * Need to refine how the scoping studies will be carried out (how we will compare sensitivity to XS libs, quadrature order, etc)
  * Make slide on follow-up work to Rachel's work at bettis? 
* Updating the gantt chart
  * I want to update this once I finish my method implementation. I'd like to go 100% on that and finish it before I give myself new deadlines.
* Productivity
  * Bullet journaling
* Savio intall
  * Building with shared libraries off
  * Building with intel, gcc, openmpi modules 
  * Built our own versions of hdf5, silo, pcre
  * Having issues with finding proper compiler libraries. Build fails with blas/lapack linking (sigh). 
* Sample problems
  * Garrett has been making good progress on the test problem cases
  * I also assigned him the topic to think about which XS libraries will be good for scoping studies

Next Week:
* This friday I have the ORNL call; hopefully we will finish up the install on savio. 
* I will be at beyond academia Mon-Tues
* Hopefully finishing up the method v1; rest of unit tests after that

Long term: 
* Finish certification as SW carpentry instructor
* Finish applying for postdocs (ANL, ORNL, LANL, INL, academic jobs)
  * Ask about Rachel sending CV to contacts
  
Other things:
* Can I put thesis data on github (.silo files)?
  * Make new repo for this for reproducibility? 
* I'm having an llvm issue on my laptop so building is sort of messed up right now (thanks to macports). 




### Update 2016/02/17

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

Other things:
* Go over new way of meeting deadlines
  * Talk about timing stuff
* Show bullet journal task management system
* Weekend thing

### Update: 2016/02/12
* Lit review additions are taking a bit longer than expected
  * Added info from papers in the results sections; elaborated more on angular methods (Mon-Weds).  
* Spent Thursday with savio debugging
  * Do we want to work on getting dev version of Scale for Marissa's runs?
  * Committing to master with install files

* Garrett Meeting: Moved due to interview
* Group Meeting: Missed
* ORNL call friday:
  * debugged why I couldn't commit
    * Seth suggests cherry picking my commits on new branch and then merging that branch. May not be an issue once I am able to commit.
    * Need to rebuild Savio without shared libraries
      * The tests that I run on 12/15 build can't find python libraries. Seth thinks this is probably a linking error. 
    * Tests can be run in parallel on an interactive node, but you must call total number of nodes when srun is enacted (see Lab Notebook)
  * got new skeleton scale from Seth

* Group Tools:
  * I volunteered for:
    *  making a travel page (this week)
    *  software carpentry page (March)
    *  getting everybody on BIDS guest list (once doodle poll results are in)
    *  paperwork/forms page (March)
    *  making directory strucutre
  * How do you want these pages organized? All in the group resources repo?
  * ILibrarian: When do you want my papers added to this? I would prefer to wait until March/April since I have a Zotero library I am very familiar with. 

### Update: 2016/02/05

* My thesis is in a rough outline form
  * I will add more detail to sections later; specifically I plan to more heavily outline the method of CADIS-OMEGA next week.
  * I am going to delay the complete outline of CADIS OMEGA theory until after I finish implementation. 
* I have been working on the background section of my thesis. 
  * Most of the variance reduction sections have the content in them that I would like, but I want to make things flow more smoothly before I ask people to review it. 
  * Right now things are fairly rough; I describe one method, then another method, then another method, with very little transition between each description.
  * I still need to work on the variance reduction background content + the adjoint background content
* Thursday meetings:
  * With Garrett: We went over more problems for Garrett to make into models. Garrett also suggested a few from his reactor experience at Westinghouse
  * Richard and I met and brainstormed a little bit about future methods -- also methods to quantify the degree of anisotropy in each cell voxel

Tasks for this upcoming week:
* Complete the method implementation
* ORNL call on Friday to make sure my testing framework is up to scratch
* Make tool to plot anisotropy of the flux in VisIT
* Figure out why the heck changing permissions for LaTeX did not help me having to sudo my builds
* Group Tools meeting on Friday

### Update: 2016/01/29

I finished the PHYSOR paper.
  This included:
  * Connecting my method with ADVANTG functionality to generate weight windows and biased source parameters.
  * Generating MCNP input problems for the method
  * Learning how to plot the data in VisIT
  * Ensuring that my data was written to .silo files for VisIT properly
  * Writing up analysis section explaining successes and issues with method

I, in the future would like to figure out a few things to make future work easier
  * Learn how to automate VisIT plotting
  * Write a function that qunatifies the degree of anisotropy of the flux in a given cell voxel (maybe max/average ratio or something). 
  * Write scripts to automate plotting of data for test suite
  * Write a script to automate test execution

Other notes:
* Garrett and I are putting a folder in the caskmodels repo to include the test problems for studying the performance Method I.
  * I think the description of this repo should be updated to be relevant to the angular hybrid methods project, rather than just the caskmodels project. 
* I need to reinstall Denovo and all of the TPLs on savio to be in the new nuclear group structure (rather than the symlink)
  * Document install process for future users on my research repo
    * Also need to make my research repo publicly accessible
  * Run all of the tests in ADVANTG and denovo to make sure they can run as expected.

Upcoming Deadlines:
  * Detailed outline of dissertation (next week, beginning)
  * Background draft I (next week, end)
  * Theory section of Method I CADIS-OMEGA (week after next)


### Update: 2015/12/15

* Updated the gantt chart, partially
* Worked on Research Statement (in the evenings this week)
* I rebuilt exnihilo with mpiexec explicitly defined
  * I'm still checking to see if all of the tests run
* The current build of Exnihilo doesn't include lava
  * In the future I need to actually build it with lava
* I'm working on writing up a set of directions for each issue I've dealt with in [Exnihilo Savio](./Exnihilo-Savio.md). This is a lame doc name and feel free to come up with a better one. 
* Tom replied about being on my committee
  * Said he is happy to do it, but also suggested Tara for professional development. Thoughts? 
* I have a call with Tara and Seth on Friday, so I'm going to work on Advantg stuff for the rest of the week.  

### Update: 2015/12/12

* This week I spent writing and working on my research, teaching, and cover letters. 
  * I'm having some trouble with phrasing of them, so I'll have to work on making it sound like it is in my voice. 
  * Teaching statement / cover letter are in first draft. Still working on Research Statement.
* For next week I need to **Update the gantt chart** to be more detailed for the upcoming months.  


### Update: 2015/12/05

* Worked with Seth and Tara to get tests passing in new build. 
  * It seems that the mpiexec is being called from `/usr/bin/sbatch`, rather than `${GRP_DIR}/TPLs/packages`
  * I need to rebuild exnihilo with this now. 
* The group dir was changed from ac_nuclear to co_nuclear
  * This affects my build of exnihilo
  * It also affects the usability of the TPLs that I built. 
  * I got Krishna to reinstate the symlink to ac_nuclear to rebuild exnihilo (since I was debugging it). 
    * When the symlink is deleted, I'll need to rebuild everything. 
    * This will probably need to be done in Jan 2016
* I got a (seemingly) complete build in /global/scratch/munkm
  * The tests don't pass yet. 
  * There are very few tests actually working. 
* I can't build in $GRP_DIR/exnihilo/builds
  * the nuclear group dir is too full right now. 
  * once we have decided what to do with the space issue, then I will build it there. 
* Krishna and/or Yong have not responded about whether their TPLs were/are built with the -fPIC flag
  * See my lab notebook for a more detailed discussion of the -fPIC flag. 
  * Seth confirmed that all TPLs need to be built with -fPIC for me to build exnihilo


### Update: 2015/11/22
**Updates:**

* I'm going create a new .md page called [Lab Notebook](./Lab-Notebook.md), where I'll record various debugging steps in detail. I think for situations, like this gcc nonsense, it will be useful for future people to avoid making my mistakes. 
* Much of my week has been going back and forth with Yong Quin and Aron Roberts on module support in Savio, and with Seth and Tara on stuff with Exnihilo. 
  * Right now I'm stuck on a compile issue that has to do with an -fPIC flag. My build fails linking some hdf5 file with the following error: 
              
        ```
        ld: /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a(H5.o): relocation R_X86_64_32 against `.rodata.str1.4' can not be used when making a shared object; recompile with -fPIC
        /global/software/sl-6.x86_64/modules/intel/2015.0.090/hdf5/1.8.13-intel-p/lib/libhdf5.a: could not read symbols: Bad value
        ```
        
  * I'm also having issues debugging the build because /tmp/ keeps filling up. Aron and Yong are aware of this issue. 

* Other than dealing with this gcc nonsense (described more in detail on the notebook page), I've gotten ADVANTG to print angular fluxes from a basic Denovo run. 

**Next Week:**

* I would like to get ADVANTG to call my python script that generates the adjusted scalar flux, and output those scalar fluxes.  
* I would like to finally get Exnihilo to build.  


### Update: 2015/07/29
**Updates:**
* I finished a rough copy of the integrating module in python.
  * Reads in tom's structures HDF5 files outputted from forward and adjoint transport 
  * performs the weighted integration to produce adjoint scalar fluxes. 
  * The adjoint scalar fluxes are then printed to a silo file with the associated geometric mesh by energy group. 
* Working on integrating the method into Denovo and advantg

* The lit review is on pause until I finish the method. Earlier this month I found a lot of relevant papers (thanks for the contributon response one), and I'm pretty happy with the start I've made, even though I don't have a good document written up yet. 

* Quals studying with Richard is going well. We're reviewing CADIS and FW-CADIS this Friday (if time permits). He is a really great person to work with.
* I'm still working on getting the 4th member for my quals committee. 
  * John Harte still hasn't confirmed whether he'll be on my committee or not. 
  * Other people haven't responded. 
    * Should I be concerned about annoying them if I keep following up? Or should I just not care? 
  * This reminds me of SpaceX!!!!!!!!!!!
* I'm reviewing policy together with Nathan. I'd like to ask your advice on our strategy for studying that topic. 
  * I took classes in both energy policy and national security policy; nathan took classes only in nonproliferation and national security. 

### Update: 2015/06/23
**Questions:**
* Do you think I should submit a paper to ICTT now that we know it will eventually be a journal publication rather than just a two page abstract? Or should we stick with the PHYSOR plan? 
* Should I make a new (public) repo for my thesis?

**Other Updates:**
* I made a pretty sizeable dent in studying transport last week
* I actually started a little early on doing lit review stuff (to be honest, I feel much more comfortable with the material on the topic, so I thought I'd do some of that). 
    * I'm meeting with the NE librarian tomorrow to try to optimize my use of library resources. 
    * Also I found an error in the library system when I was trying to request some older documents from the NRLF, so that's when I decided to make a meeting.  


### Update: 2015/01/26
**Goals from last time:**
* **Done** Finish M&C first draft
* **Done** Submit paper to M&C conference
* **Done** Read two more papers for lit review
* **Not Done** Get master document of lit review to compile correctly.

**Other Updates:**
* Ordered computer, as you know
* e-mailed karl and leah the M&C paper. Karl caught some issues in the references section (which I never really looked over, so that is my bad. 
* Sorting out pay issues with ERSO/HR 
* Worked more on M&C paper revisions (like updating the plots)
* Reorganized my calendar to smaller 1-2 hour chunks of focused work

**Questions:**
* How do we want to organize the Friday discussion?
  * I would like to focus my work this week towards that meeting and what we will discuss. 

**Goals for Next week:**
* Read 3-4 papers for next week (~1 per day) 
* Reviewing some math stuff from linear algebra to enhance my understanding of all of my papers

### Update: 2014/12/30
**Goals from last time:**
* **Done** Get working draft of M&C paper done
* **Not Done** Read one more paper for lit review. 
* **Not Done** Get master document of lit review to compile correctly.

**Goals for next week:**
* Finish M&C first draft
* Submit paper to M&C conference
* Read two more papers for lit review
* Get master document of lit review to compile correctly. 

### Update: 2014/12/22
**Goals from last time:**
* **Not uploaded, since I'm still fiddling with the master document.** Continue uploading notes from papers
* **Not done** (I only read two) Read three more papers for lit review
  * The Somasundaram and Palmer paper
  * Becker and Larsen paper
* **Done** Study for final
* **Not done** (This took a backseat to studying for my final last week.) Work on draft/revisions of mars rover paper

**Goals for next time:**

I'd like to go a little lighter on my workload this upcoming stretch, because my parents' house is filled with people and I'm going to have very little privacy to work and/or focus. I'd also like to take a few full days off. Until our next update I'll do the following:  
* Try to get good draft of mars rover paper finished by M&C deadline. I'm now uncomfortably behind on this, and I need to put it first. 
* Read one more paper for lit review
* Get master document of lit review to compile correctly

### Update: 2014/12/10
**Goals from last week:**
* **Done** I have a paper for my policy class due and a homework due for the same class.
  * I plan on focusing on these for the rest of the week.
* **Done** I will upload more papers on the weekend (and get a current bibtex file).
* **Done** After the weekend I'd like to read at least two more papers. 
* **In Progress** Get a good draft of the mars rover work sent out by the end of next week (~12/12).

**Upcoming week:**
* Continue uploading notes from papers
* Read three more papers for lit review
* Study for final
* Work on draft/revisions of mars rover paper

### Update: 2014/12/03
**Goals from Last week:**
* **done, but not as much as I'd like** Continue with mars rover neutronics report
* **Not done** Read a few more papers for the lit review. 
* **Done** Continue reading Importance: The Adjoint
* **Done** Add .tex files to github repo and make PDFs linkable from my research page
  * I've only uploaded a few so far, but I am happy with the linking and how things turned out. More will be merged later this week. 
  * My plan is to have a full document compiling all the notes with a proper bibtex document added too. Does this seem like a good plan? 

**Upcoming week:**
* I have a paper for my policy class due and a homework due for the same class. 
  * I plan on focusing on these for the rest of the week. 
* I will upload more papers on the weekend (and get a current bibtex file).
* After the weekend I'd like to read at least two more papers. 
* Get a good draft of the mars rover work sent out by the end of next week (~12/12).

**Other things:**
* Should I go to that NSSC open source tools thing? I am not sure it is super relevant to my research area. 

### Update: 2014/11/24
**Updates from last week:**
* The group meeting went really well. Professor Greenspan had a lot to say about what we've done so far, and was very happy to provide some resources based on past work that he has done. 
  * I'm actually including several of these in the background and 'future work' sections of the paper that we're writing.
  * Paper is being written in LaTeX. woo! (my original outline was not) 
  * I plan on getting you a draft of the neutronics-specific report that we're sending to ESA in the next two weeks. Are you okay being on the report **and** the paper (both neutronics-specific)? 
* Talked to Leah on the Mars Rover work for the firt time in a few weeks. We now have all of our data completed, so it's just writing at this point.
  * After ANS I ran a few different fissionable materials, like we talked about, and have results on the criticality as a funciton of mass for several materials.
* Went back to the hybrid methods lit review. Got to read two papers this week. 

**Upcoming week**
* Continue with mars rover neutronics report
* Read a few more papers for the lit review. 
* Continue reading Importance: The Adjoint

### Update: 2014/10/29
**Goals from last week:**
* **Started** Continue on reviewing math methods
* **Started** Get stuff written up in LaTeX

**Other things:**
* When I get rover paper written up, where/how should I submit it
  * Is it better to submit papers to conferences? How do I balance publications in peer-revieiwed journals with conferences? 
  * Feeling a little lost in where I'm going right now
* Tried to do a big push this week to get some rover stuff out of the way.
  * Outlined neutronics paper for rover project
    * Focus of this paper is the reasons we made design-specific decisions neutronically
  * Sent all data to my colleagues.  

**Goals for next week:**
* Make poster for ANS
* Actually get LaTeX writeups on github page
* Start doing some C++ tutorials
* Go back to reading papers
* Finish giant HW assignment for my class. 

### Update: 2014/10/22
**Goals from last week:**
* **Done** Starting on math backrgound
* **Not Done** Papers (although we never really decided this was a goal anyways)

**Other things:**
* I started reviewing some linear algebra and methods stuff. 
* I have a midterm this week, so I've mostly been focusing on that. 
* Should I start thinking of conferences to go to? 

**Goals for next week:**
* Continue on reviewing math methods
* Get stuff written up in LaTeX


### Update: 2014/10/15

**Goals from last week:**
* **No, still working on LateX writeup** Finish ORNL report.
* **Done** Read 2 more papers from shared dropbox folder. What are your thoughts on these three (?):
  * **Read** Gol'din
  * **Read** Miften and Larsen
    * Supplemented these with summary section II.G from M.L. Adams and E.W. Larsen book you provided
  * **Not Read** E.T. Jaynes (if I can get to it!) 

**Lit Review Work:**
* Acquired Hendricks (1982) **Read**
* Acquired Booth (1982) **Read**

**Other Things:**
* Sumbitted abstract and registered for ANS winter meeting
* Room paid for by Prof. Vujic, Air travel paid by ANS
* I have been going back through the math theory stuff we talked about last week. Thank you for providing these.  

**Goals for Next Week**
* Personally, I would like to focus on some of the math background. What are your thoughts on this? 
* Paper suggestions? 

### Update: 2014/10/08

**Goals from last week:**
* **No** Finish ORNL report. (Done reading, but still going back and taking notes.)
* **Done** Skim associated papers that I downloaded that were referenced in Larsen & Cooper and FW CADIS papers
* **Done** Respond to prof. Greenspan's comments on my master's report.
* **Started** Outline neutronics-specific paper for rover project

**Lit Review Work:**
* I'm still working on the .tex document outlines for each paper I've read. 

**Goals for Next Week:**
* Finish ORNL report. 
* Read 2 more papers from shared dropbox folder. What are your thoughts on these three (?):
  * Gol'din
  * Miften and Larsen
  * E.T. Jaynes (if I can get to it!) 

### Update: 2014/10/01

**Lit Review Work:**
* Read through AVATAR paper, took notes (I'll add a link to this later)
* Started reading ORNL report.

**Git Repo:**
* For the moment, I've made my own git repo under a really generic title. I think this way after I move beyond the lit review, this can still be the place that I put weekly updates and whatnot. I have a few thoughts:
  *For my lit review notes, should I put them in your AngularHybridLitReview folder? 
  * If not, I could:
    * Make a subfolder in this directory that has its own markdown file for each paper, then link each paper to a top 'lit review' page
    * Put all of my notes in a lit review page. 
 
**Other Work:**
* Skype meeting with Leah went well. We have a major start on the geology-specific paper. I should be starting my outline for the neutronics paper this weekend. 

**Goals for next week:**
* Finish ORNL report. 
* Skim associated papers that I downloaded that were referenced in Larsen & Cooper and FW CADIS papers
* Respond to prof. Greenspan's comments on my master's report. 
* Outline neutronics-specific paper for rover project 
