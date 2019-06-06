# VIRTUAL MACHINE SPLC19.ova DOWNLOAD LINK: <https://drive.google.com/open?id=1fiQWt_E_N5KTQBBVXSvFxelSypzf_1tA>

# Sampling Product Configurations Of Feature Models That Have Numerical Features (Artifact)

This repository consist of a VirtualBox *Virtual Machine* (VM) pre-built and pre-configured **to** **re-create** the experiments of the research paper https://doi.org/10.1145/3336294.3336297, and, as well, **to re-use** it with different feature models and/or data-sets.

​	Analyses of *Software Product Lines* (SPLs) rely on automated solvers to navigate complex dependencies among boolean features and find legal configurations. Often these analyses do not support numerical
features with constraints because propositional formulas use only Boolean variables. While other automated solvers can represent numerical features natively, they are limited in their ability to count
and *Uniform Random Sample* (URS) configurations, which are key operations to derive unbiased statistics on configuration spaces. 

​	*Bit-blasting* is a technique to encode numerical constraints as propositional formulas. We use bit-blasting to encode boolean and numerical constraints so that we can exploit existing #SAT solvers to count and URS configurations. 

​	Compared to state-of-art Satisfiability Modulo Theory and Constraint Programming solvers, our approach has two advantages: 1) faster and more scalable configuration counting and 2) reliable URS of SPL configurations. Our work can be **re-used** to extend prior SAT-based SPL analyses to support numerical features and constraints.

​	This *README* provides the detailed explanations and technical steps to fast and easily make use of our approach. **<u>A VM is provided due to the amount of knowledge and time necessary to configure and run the third-party tools**</u>. Additionally, a Linux operating system is mandatory to run all the tools, while a VM runs on almost any operating system and/or hardware. The VM includes the VirtualBox Guest Tools to allow direct interaction and responsiveness of the VM main *Desktop*.

## Artifact Third-Party Content

- Lubuntu 18.04 LTS x86_64 operating system ([Lubuntu](https://lubuntu.net/)).
- Python Interpreter version 3.7 ([Python3.7](https://www.python.org/)).
- Oracle's open-source Java Development Kit ([Open-JDK](https://openjdk.java.net/)).
- Clafer Instance Generator 0.45 [(ClaferIG).](https://github.com/gsdlab/claferIG)
- Z3 theorem prover SMT solver for Python [(Z3py)](https://github.com/Z3Prover/z3).
- Model counting SAT solver ([#SAT](https://github.com/marcthurley/sharpSAT)).
- JetBrains 2019.1 Python IDE ([Pycharm](https://www.jetbrains.com/pycharm/)).
- URS, *Statistical Random Sampling* (SRS) and *Recursive Random Sampling* (RRS) tools from the co-authors of the University of Texas at Austin et al. ([SMARCH](https://apps.cs.utexas.edu/apps/tech-reports/171355)).
- Kolmogorov-Smirnov Test web-service ([KS-Test](https://dl.acm.org/citation.cfm?id=3106273)).
- KS-Table: ![KS-Table](https://i0.wp.com/www.real-statistics.com/wp-content/uploads/2012/11/one-sample-ks-table.png?w=514)
- Mann-Whitney U Test ([MWU-Test](https://www.socscistatistics.com/tests/mannwhitney/default2.aspx)).

## Artifact New Content

- Clafer, Z3 and DIMACS numerical feature models of published SPLs ([Dune,HSMGP, HiPAcc, Trimesh](https://dl.acm.org/citation.cfm?id=3106273), [axTLS, Fiasco, uClibc-ng](https://www.springer.com/gp/book/9783642375200)).
- Python scripts to time and model-count Clafer, Z3 and DIMACS models.
- Python script to, making use of Z3 alternative functionality, transform numerical features modeled in Z3 into Tseitin-CNF DIMACS format using Bit Blasting. **It supports:**
  - First order logic features and operations (*AND*, *OR*, *XOR*...).
  - Booleans and signed and unsigned integers.
  - Arithmetic operations as: equality (*=*), inequalities (*<>*, *>*, *≥*,...), addition (*+*), etc.
  - Composed or multiple functions (e.g., *(a + b) > c*)
- Python script to time and obtain a certain number of random solutions (random sampling).  
- Python script to rank, estimate, and compare both ranks of a set of random solutions. Each set of N solutions is measured and obtained from a different reasoner.
- Intermediate files as set of samples, ranks, comparisons, solutions, etc.
- Uniform data distribution test Python scripts.

## Requirements

- A PC, laptop or notebook with at least 4GB of RAM and 5GB of free space.
- Oracle VM VirtualBox installed ([Downloads](https://www.virtualbox.org/wiki/Downloads)).
- Intel VT-x or AMD-V 'Active' in the motherboard BIOS settings, necessary for x86_64 VMs ([+INFO](https://www.youtube.com/watch?v=g8-go0RFPjc)).

## Usage

The first step is to load the downloaded VM into VirtualBox.  We just need to click *File->Import Appliance* and search for *SPLC19VM.ova*. After that, we just *click on it* *and press Start*.

In case it is needed, Lubuntu:

- User: caosd
- Password: splc19
- Sudoers password: splc19

After the desktop has appeared, we recommend to resize and maximize the VM view, so it automatically adapts to the desired screen size.

In the main Desktop we find:

- Empty *Trash*
- Folder *featuremodels* where all NFMs in the different formats (Clafer, DIMACS and Z3) are located.
- Folder *samples* where 100, 300 and 500 pre-computed sampling files for each solver and feature model are located.
- Folder *UFscripts* where a sort of scripts for model count and sampling with sharpSAT are located.
- Launcher *PyCharm* Python IDE 2019.1.
- Executable *Clafer-tools-0.4.5* and as examples *clafercommands.txt* 
- Launcher *LXTerminal*.



**RQ1: "*How many bits per Numerical Feature are feasible with Bit-Blasting?*"**

The steps to re-create RQ1 are:

1. Represent **any feature model** into DIMACS format:

   1. First we need a CNF representation of the Boolean part of a feature model. For the seven models evaluated. The just Boolean models can be found at: */home/caosd/Desktop/featuremodels/sat/Boolean*

   2. We also need the definitions of the numerical features, and their constraints with other features, whether they are Boolean or not. They can be found at: */home/caosd/Desktop/featuremodels/sat/Numerical*

      NOTE: For relatively small SPLS , they can be completely modeled (Boolean and numerical features) if advanced Z3 modeling skills are available and the system is computationally powerful. In our case, FSE models followed this shortcut, and they can be found at: */home/caosd/Desktop/featuremodels/sat/Shorcut*

   3. We need to transform the numerical definitions into CNF, and then, append them to the Boolean models of step 1.1. For that, we open PyCharm the main project (splc19) should be automatically opened, with the address */home/caosd/PycharmProjects/splc19*. There, we can open the script named *Z3toCNF.py*, which transforms Z3 models into Tseitin-CNF DIMACS format. Some caveats are:

      1. *initvar = X* sould be changed to the last feature number of the Boolean DIMACS model. X is by default 1, as DIMACS variables start in 1.
      2. *stringvars = List.Names|| vars =  BitVecs(Spaced.NAMES, N)...* should include the numerical features name (NAMES), and the total number of them (N)
      3. g.add(...) should include the constraints separated by regular commas. Examples of constraints and their possibilities are added in the script Header. One working example is ready for testing.

   4. The end-results (Boolean CNF + appended numerical features CNF) of the 7 SPLs are pre-computed and accesible at: */home/caosd/Desktop/featuremodels/sat/DIMACS*

2. Count the number of CNF clauses of the DIMACS models based on the number of bits (range) of the numerical features. The numerical features are located in the header of each DIMACS model.

3. Measure the average time to count all the valid configurations by sharpSAT 97 times. For this, there are Bash scripts for each model at: */home/caosd/Desktop/UFscripts/SharpSATCounting* as well as a generic one (*generic.sh*). In case of the specific ones, you must run them using *Time* at the LXTerminal as:  *time ./NAME.sh*. In case of the generic one, two input parameters must be declared, number of counting repetitions and the location of the DIMACS model, for example:  *./generic.sh 97 /home/caosd/Desktop/featuremodels/sat/DIMACS/axtls_2_1_4.dimacs*.

4. Measure the time to URS a configuration by SMARCH. For this, there are Bash scripts for each model and number of samples tested in the research (i.e., *100*, *300*, *500*) at: */home/caosd/Desktop/UFscripts/SharpSATSampling* as well as a generic one (*generic.sh*).  In case of the specific ones, you must run them accessing the folder with the name of the feature model and execute the desired number of samples in the LXTerminal:  *./NUMBER_OF_SAMPLES.sh*. In case of the generic one, two input parameters must be declared, number of samples and the name of the DIMACS model located at the folder *featuremodels*, for example:  *./generic.sh 100 Dune*. Every execution result is equally available  at */home/caosd/samples/sat/*. 



**RQ2: "*Is configuration counting efficient for Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ2 are:

1. First, as it is a model counting performance comparison, we need an SPL modeled in Clafer, Z3 and DIMACS. You can find our 7 evaluated SPLs modeled in the three alternatives in the folder: */home/caosd/featuremodels*. For convenience, Z3 models are as well found in the PyCharm project *spl1c19*, under the folder Z3. They are all identified by their concrete names. In case of external models that are just in DIMACS format, we two scripts, one to transform DIMACS into Clafer, and another DIMACS into Z3. They are located in the *PyCharm splc19 project*, folder *DIMACStransformations*. The main caveat is *dimacsname = NAMEOFSPL* and that DIMACS headers must be moved to a parallel file *NAMEOFSPLvars.dimacs*.
2. SharpSAT timing and counting has already been performed in RQ1.
3. Measure the average time to count all the valid configurations by Clafer 97 times. For this, there are Bash scripts for each model at: */home/caosd/Desktop/UFscripts/ClaferCounting* as well as a generic one (*generic.sh*). In case of the specific ones, you must run them using *Time* at the LXTerminal as:  *time ./NAME.sh*. In case of the generic one, two additional input parameters must be declared, number of counting repetitions and the name of the DIMACS model, for example:  *./generic.sh 97 axtls*.
   1. NOTE: Clafer models (*txt* format) are compiled as an intermediate step creating *.js* format models. Both, .txt and .js, are located in /home/caosd/Desktop/featuremodels/clafer
4. Measure the average time to count all the valid configurations by Z3 SMT solver 97 times. For this, we need to launch *PyCharm* found in the *Desktop*. Open the models identified by the *name.py* under the *z3* folder of the *splc19* project, and just run them individually with *RightClick->Run*.
   1. NOTE: KConfig models (Axtls, Trimesh and uClibc) where considered a timeout due to how long it takes to count configurations with Z3 SMT solver with them (i.e., years). We kindly advise not to perform model counting with such large models.
   2. NOTE: Do not modify Z3 models in this step, we will explain the code further in the next RQs. By default are RQ2 ready. We remind to you that untouched copies of them are found in /home/caosd/Desktop/featuremodels/Z3

**RQ3: "*Is URS efficient for Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ3 are:

1. We need to time and obtain a serie of 100, 300 and 500 random samples (**called random sampling**) for each feature model and each reasoner. Each system has its caveats, and some of the mixes of SPL/Number of samples/Reasoner can take an hour, and in a VM much more. In case to avoid tedious computation, the results (i.e., the samples) are already pre-computed at ***/home/caosd/Desktop/samples*** differentiated by *reasoner/number_of_samples/SPLNAME.txt*. There are intermediate calculations folders (e.g., */clafer_intermediate*) as the standard output of the solvers need sometimes to be transformed in a more programming-friendly format.

   *NOTE*: SMARCH does the sampling of SharpSAT, from now on, we talk about SMARCH, and not SharpSAT.

2. **Random Sample with Clafer**: For this, there are Bash scripts for each model and each set of samples at: */home/caosd/Desktop/UFscripts/ClaferSampling* as well as a generic one (*generic.sh*). In case of the specific ones, you must run them using *Time* at the LXTerminal as:  *time ./NAME.sh*. In case of the generic one, two additional input parameters must be declared, number of samples and the name of the DIMACS model, for example:  *./generic.sh 100 Trimesh*. NOTE: The total time must be divided between the number of samples.

3. **Rank Clafer samples and check KS-Test**: 

   1. **Not colossal models** (e.g., DuneSystem, Trimesh, HiPAcc, HSMGP): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. We need to open *sortfullranks.py* under the *ranksampling* directory of the *splc19* project. This script sort the ranks of an SPL depending on their performance goal. Just the name of the model must be change. The unsorted configurations are located at */home/caosd/featuremodels/FSE_models_performance/not_ranked/*, however, the pre-computed sorted configurations are located at */home/caosd/featuremodels/FSE_models_performance/ranked/*
      2. Open *readclaferFSE.py* under the *ranksampling* directory of the *splc19* project. This scripts transforms Clafer regular output into a more programming-friendly file. Caveat are *name = "NAMEOFSPL"* and *loopa = [NUMBER, OF, SAMPLES]*. The inputs are the previous point files, the outputs are localted (and pre-computed) at */home/caosd/samples/clafer_programming_friendly/*.
      3. Open *ranksamples.py* under the *ranksampling* directory of the *splc19* project. This script uses  the two previous files to output the ranks sorted with their respective performance metrics. Caveat are *name = "NAMEOFSPL"*, *loopa = [NUMBER, OF, SAMPLES]*, *reasoner = "clafer"*, and *spltype = "FSE"*. The outputs (and pre-computed) are located at: */home/caosd/samples/clafer_ranked*
      4. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if ti is below critical value of 95% compared to a KS Table).
   2. **Colossal models** (e.g., axtls_2_1_4, fiasco_17_10, uClibc-ng_1_0_29): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. Open *readclaferKConfig* under the *main directory* of the *splc19* project. This scripts transforms Clafer regular output into a more programming-friendly file. Caveat are *name = "NAMEOFSPL"* and *loopa = [NUMBER, OF, SAMPLES]*. The inputs are the previous point files, the outputs are localted (and pre-computed) at */home/caosd/samples/clafer_programming_friendly/*.
      2. Open *HCS_Optimizer/randtest.py* in the *PyCharm splc19 project*. Main code caveats are *modelname = "SPLNAME"*, *tool = "clafer"* and *numsamples = NUMBEROFSAMPLES* (which in our evaluation are *100*, *300* and *500*. The inputs are the *model.dimacs* and the *model.json* that contains the performance metrics. We recall that the models are located at */home/caosd/featuremodels/sat/DIMACS/* , the performance metrics at */home/caosd/featuremodels/KConfig_models_performance/not_ranked/* and the samples programming friendly at */home/caosd/samples/{tool}_programming_friendly/*.
      3. The lines between *#START SMARCH SAMPLING:* and *#END SMARCH SAMPLING:* must be commented. The lines between *#START Clafer OR Z3 ranking:* and *#END Clafer OR Z3 ranking:* must be uncommented
      4. The *Total Time* output must be divided by the *numsamples* as the individual timings has considered some overhead.
      5. In the last set of outputted data we can see a table with sorted *Expected Rank*, and *Actual Rank* of each configuration.
      6. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if it is below critical value of 95% compared to a KS Table).

4. **Random Sample with Z3**:

   1. For this, we need to launch *PyCharm* found in the *Desktop*. Open the models identified by the *name.py* under the *z3* folder of the *splc19* projec. For each of them, the caveats are:
      1. Uncomment:
         1. Each commented *import*.
         2. The two *s.set()* lines.
         3. All lines starting wit *file...*.
         4. Line *if numbersolutions == maxsol: break*
      2. Adjust *maxsol=NUMBEROFSAMPLES*, which in our evaluation are *100*, *300* and *500*.
      3. Adjust *numberoftests=REPETITIONS*, which in our evaluation are 97 times.
         1. NOTE: Colossal models (*axtls*, *fiasco* and *uClibc*) are evaluated individually as running it many times in a loop can cause the VM to run out of memory and crash the system. In other words, they do not have this option.
      4. Run them individually with *RightClick->Run*
      5. The timings of each run, and the averaged time of all of them are printed last in the terminal.
   2. The samples would be located (and are pre-computed) at */home/caosd/samples/z3/*.

5. **Rank Z3 samples and check KS-Test**:

   1. **Not colossal models** (e.g., DuneSystem, Trimesh, HiPAcc, HSMGP): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. We need to open *sortfullranks.py* under the *ranksampling* directory of the *splc19* project. This script sort the ranks of an SPL depending on their performance goal. Just the name of the model must be change. The unsorted configurations are located at */home/caosd/featuremodels/FSE_models_performance/not_ranked/*, however, the pre-computed sorted configurations are located at */home/caosd/featuremodels/FSE_models_performance/ranked/*.
         1. **NOTE:** If the similar Clafer case study has been followed, this step is already done.
      2. Open *readz3.py* under the *ranksampling* directory of the *splc19* project. This scripts transforms Z3 regular output into a more programming-friendly file. Caveat are *name = "NAMEOFSPL"*, *fileextension = "csv"* in case of non-colossal SPLs, * = "txt"* in case of colossal ones, and *loopa = [NUMBER, OF, SAMPLES]*. The inputs are the previous point files, the outputs are localted (and pre-computed) at */home/caosd/samples/z3_programming_friendly/*.
      3. Open *ranksamples.py* under the *ranksampling* directory of the *splc19* project. This script uses  the two previous files to output the ranks sorted with their respective performance metrics. Caveat are *name = "NAMEOFSPL"*, *loopa = [NUMBER, OF, SAMPLES]*, *reasoner = "z3"*, and *spltype = "FSE"*. The outputs (and pre-computed) are located at: */home/caosd/samples/z3_ranked*
      4. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if ti is below critical value of 95% compared to a KS Table).
   2. **Colossal models** (e.g., axtls_2_1_4, fiasco_17_10, uClibc-ng_1_0_29): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. Open *readz3.py* under the *ranksampling* directory of the *splc19* project. This scripts transforms Z3 regular output into a more programming-friendly file. Caveat are *name = "NAMEOFSPL"*, *fileextension = "csv"* in case of non-colossal SPLs, * = "txt"* in case of colossal ones, and *loopa = [NUMBER, OF, SAMPLES]*. The inputs are the previous point files, the outputs are localted (and pre-computed) at */home/caosd/samples/z3_programming_friendly/*.
         1. **NOTE**:  If the not colossal case study has been followed, this step is already done.
      2. Open *HCS_Optimizer/randtest.py* in the *PyCharm splc19 project*. Main code caveats are *modelname = "SPLNAME"*, *tool = "z3"*  and *numsamples = NUMBEROFSAMPLES* (which in our evaluation are *100*, *300* and *500*. The inputs are the *model.dimacs* and the *model.json* that contains the performance metrics. We recall that the models are located at */home/caosd/featuremodels/sat/DIMACS/* , the performance metrics at */home/caosd/featuremodels/KConfig_models_performance/not_ranked/* and the samples programming friendly at */home/caosd/samples/{tool}_programming_friendly/*.
      3. The lines between *#START SMARCH SAMPLING:* and *#END SMARCH SAMPLING:* must be commented. The lines between *#START Clafer OR Z3 ranking:* and *#END Clafer OR Z3 ranking:* must be uncommented
      4. The *Total Time* output must be divided by the *numsamples* as the individual timings has considered some overhead.
      5. In the last set of outputted data we can see a table with sorted *Expected Rank*, and *Actual Rank* of each configuration.
      6. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if it is below critical value of 95% compared to a KS Table).

6. **Random Sample with SMARCH, rank the samples and check KS-Test**:

   1. **Not colossal models** (e.g., DuneSystem, Trimesh, HiPAcc, HSMGP): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. Open *HCS_Optimizer/evaluation.py* in the *PyCharm splc19 project*. Main code caveats are *modelname = "SPLNAME"* and *numsamples = NUMBEROFSAMPLES* (which in our evaluation are *100*, *300* and *500*. The inputs are the *model.dimacs* and the *model.csv* that contains the performance metrics. We recall that the models are located at */home/caosd/featuremodels/sat/DIMACS/* and the performance metrics at */home/caosd/featuremodels/FSE_models_performance/not_ranked/* .
      2. The *Total Time* output must be divided by the *numsamples* as the individual timings has considered some overhead.
      3. In the last set of outputted data we can see a table with sorted *Expected Rank*, and *Actual Rank* of each configuration.
      4. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if it is below critical value of 95% compared to a KS Table).
   2. **Colossal models** (e.g., axtls_2_1_4, fiasco_17_10, uClibc-ng_1_0_29): For this, we need to launch *PyCharm* found in the *Desktop*. 
      1. Open *HCS_Optimizer/randtest.py* in the *PyCharm splc19 project*. Main code caveats are *modelname = "SPLNAME"* and *numsamples = NUMBEROFSAMPLES* (which in our evaluation are *100*, *300* and *500*. The inputs are the *model.dimacs* and the *model.json* that contains the performance metrics. We recall that the models are located at */home/caosd/featuremodels/sat/DIMACS/* and the performance metrics at */home/caosd/featuremodels/KConfig_models_performance/not_ranked/* .
      2. The lines between *#START SMARCH SAMPLING:* and *#END SMARCH SAMPLING:* must be uncommented. The lines between *#START Clafer OR Z3 ranking:* and *#END Clafer OR Z3 ranking:* must be commented
      3. The *Total Time* output must be divided by the *numsamples* as the individual timings has considered some overhead.
      4. In the last set of outputted data we can see a table with sorted *Expected Rank*, and *Actual Rank* of each configuration.
      5. We need to run the KS-Test. For this, open Firefox web-explorer, access *https://www.wessa.net/rwasp_Reddy-Moores%20K-S%20Test.wasp* and paste each case study from the previous point to check if it pass it (if it is below critical value of 95% compared to a KS Table).

**RQ4: "*Can existing SAT-analyses of SPLs use Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ4 are:

> FSE2015

1. Go to HCS_Optimizer/search.py
2. Set target, file path, rep=97, n=20, rec=-1
3. Run the program
4. Output is 6 column data for table 7:
   C1: average all rows for N
   C2: average all rows and derive pSRS (see footnote 10)
   C3: average all rows for rSRS
   C4: average all rows for rURS
   C5: average all rows for rBetter
   C6: average all rows for pURS
5. For rtest, get C3 and C4, run Mann-Whitney U test (https://www.socscistatistics.com/tests/mannwhitney/default2.aspx)
6. For ptest, get C2 and C6, run Mann-Whitney U test 

> Setup for Kconfig

1. Clone https://github.com/paulgazz/kconfig_case_studies, follow instructions on readme.md, remove bugs folder
2. Put buildSamples.sh into kconfig_case_studies folder (repo root)
3. run vagrant up inside kconfig_case_studies (always before RQ4)
4. Modify BUILD variable path at HCS_Optimizer/kconfigIO.py line 10
5. Modify self.wdir at HCS_Optimizer/evaluation.py line 19 to have correct path to kconfig_case_studies repo

> Kconfig

1. Set target, file path, rep=25, n=20, rec=-1 to get SRS data
2. Set dimacs and const path
3. change eval to "Kconfig(target, features, wdir)"
4. comment out lines 188 to 202, uncomment line 186
5. Ouput is 1 column data: average to get pSRS
6. n=200, rec=1 to get URS data
7. Ouput is 1 column data: average to get pURS
8. For pTest, SRS data and URS data for Mann-Whitney U test

## References

- [Lubuntu](https://lubuntu.net/)
- [Python3.7](https://www.python.org/)
- [Open-JDK](https://openjdk.java.net/)
- [ClaferIG](https://github.com/gsdlab/claferIG)
- [Z3py](https://github.com/Z3Prover/z3)
- [#SAT](https://github.com/marcthurley/sharpSAT)
- [Pycharm](https://www.jetbrains.com/pycharm/)
- [SMARCH](https://dl.acm.org/citation.cfm?id=3106273)
- [KS-Test](https://dl.acm.org/citation.cfm?id=3106273)
- [MWU-Test](https://www.socscistatistics.com/tests/mannwhitney/default2.aspx)

## Authors

1. **Daniel-Jesus Munoz**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
2. **Jeho Oh**: Department of Computer Science Austin, Texas, USA
3. **Mónica Pinto**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
4. **Lidia Fuentes**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
5. **Don Batory**: Department of Computer Science Austin, Texas, USA
