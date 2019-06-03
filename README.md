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
- Mann-Whitney U Test ([MWU-Test](https://www.socscistatistics.com/tests/mannwhitney/default2.aspx)).
- more ([more](https://)).

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
- more ([more](https://)).

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

1. Represent any feature model into DIMACS format:

   1. First we need a CNF representation of the Boolean part of a feature model. For the seven models evaluated. The just Boolean models can be found at: */home/caosd/Desktop/featuremodels/sat/Boolean*

   2. We also need the definitions of the numerical features, and their constraints with other features, whether they are Boolean or not. They can be found at: */home/caosd/Desktop/featuremodels/sat/Numerical*

      NOTE: For relatively small SPLS , they can be completely modeled (Boolean and numerical features) if advanced Z3 modeling skills are available and the system is computationally powerful. In our case, FSE models followed this shortcut, and they can be found at: */home/caosd/Desktop/featuremodels/sat/Shorcut*

   3. We need to transform the numerical definitions into CNF, and then, append them to the Boolean models of step 1.1. For that, we open PyCharm the main project (splc19) should be automatically opened, with the address */home/caosd/PycharmProjects/splc19*. There, we can open the script named *Z3toCNF.py*, which transforms Z3 models into Tseitin-CNF DIMACS format. Some caveats are:

      1. *initvar = X* sould be changed to the last feature number of the Boolean DIMACS model. X is by default 1, as DIMACS variables start in 1.
      2. *stringvars = List.Names|| vars =  BitVecs(Spaced.NAMES, N)...* should include the numerical features name (NAMES), and the total number of them (N)
      3. g.add(...) should include the constraints separated by regular commas. Examples of constraints and their possibilities are added in the script Header. One working example is ready for testing.

   4. The end-results (Boolean CNF + appended numerical features CNF) of the 7 SPLs are pre-computed and accesible at: */home/caosd/Desktop/featuremodels/sat/DIMACS*

2. Count the number of CNF clauses based on the number of bits (range) of the numerical features.

3. Measure the time to count all the valid configurations by sharpSAT.

4. Measure the time to URS a configuration by SMARCH.

   1. *2, 3, and 4* are all done with the script *evaluation.py* fount at: */home/caosd/PycharmProjects/splc19/HCS_Optimizer*
   2. To perform the measurement the following pieces of codes must be active, and the rest as a comment. SharptSAT and the rest of the tools are already configured in the script.
   3. Some caveats are:
      1. *target = "X"*  where X is the name of the SPL. The example alternatives are available as a comment in the script's header.
      2. *tool = "Y"* where Y is sharpSAT in this case.
      3. *size = N* where N is ... (JEHO)



**RQ2: "*Is configuration counting efficient for Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ2 are:



**RQ3: "*Is URS efficient for Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ3 are:



**RQ4: "*Can existing SAT-analyses of SPLs use Bit-Blasted Propositional Formulas?*"**

The steps to re-create RQ4 are:



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
- more ([more](https://)).

## Authors

1. **Daniel-Jesus Munoz**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
2. **Jeho Oh**: Department of Computer Science Austin, Texas, USA
3. **Mónica Pinto**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
4. **Lidia Fuentes**: CAOSD, Dpt. LCC, Universidad de Málaga, Andalucía Tech, Spain
5. **Don Batory**: Department of Computer Science Austin, Texas, USA
