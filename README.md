# Sampling Product Configurations Of
Feature Models That Have Numerical Features (Artifact)
This repository consist of a VirtualBox *Virtual Machine* (VM) pre-built and pre-configured **to** **re-create** the experiments of the research paper https://doi.org/10.1145/3336294.3336297, and, as well, **to re-use** it with different feature models and/or data-sets.

Analyses of *Software Product Lines* (SPLs) rely on automated solvers to navigate complex dependencies among boolean features and find legal configurations. Often these analyses do not support numerical
features with constraints because propositional formulas use only Boolean variables. While other automated solvers can represent numerical features natively, they are limited in their ability to count
and *Uniform Random Sample* (URS) configurations, which are key operations to derive unbiased statistics on configuration spaces. 

​	*Bit-blasting* is a technique to encode numerical constraints as propositional formulas. We use bit-blasting to encode boolean and numerical constraints so that we can exploit existing #SAT solvers to count and URS configurations. 

​	Compared to state-of-art Satisfiability Modulo Theory and Constraint Programming solvers, our approach has two advantages: 1) faster and more scalable configuration counting and 2) reliable URS of SPL configurations. Our work can be **re-used** to extend prior SAT-based SPL analyses to support numerical features and constraints.

​	This *README* provides the detailed explanations and technical steps to fast and easily make use of our approach. <u>A VM is provided due to the amount of knowledge and time necessary to configure and run the third-party tools</u>. Additionally, a Linux operating system is mandatory to run all the tools, while a VM runs on almost any operating system and/or hardware. The VM includes the VirtualBox Guest Tools to allow direct interaction and responsiveness of the VM main *Desktop*.

## Artifact Third-Party Content

- Lubuntu 18.04 LTS x86_64 operating system ([Lubuntu](https://lubuntu.net/)).
- Python Interpreter version 3.7 ([Python3.7](https://www.python.org/)).
- Oracle's open-source Java Development Kit ([Open-JDK](https://openjdk.java.net/)).
- Clafer Instance Generator 0.45 [(ClaferIG).](https://github.com/gsdlab/claferIG)
- Z3 theorem prover SMT solver for Python [(Z3py)](https://github.com/Z3Prover/z3).
- Model counting SAT solver ([#SAT](https://github.com/marcthurley/sharpSAT)).
- JetBrains 2019.1 Python IDE ([Pycharm](https://www.jetbrains.com/pycharm/)).
- URS, *Statistical Random Sampling* (SRS) and *Recursive Random Sampling* (RRS) tools from the co-authors of the University of Texas at Austin et al. ([SMARCH](https://dl.acm.org/citation.cfm?id=3106273)).
- Kolmogorov-Smirnov Test web-service ([KS-Test](https://dl.acm.org/citation.cfm?id=3106273)).
- more ([more](https://)).

## Artifact New Content
* Clafer, Z3 and DIMACS numerical feature models of published SPLs ([FSE'15]([KS-Test](https://dl.acm.org/citation.cfm?id=3106273)), [KConfig](https://www.springer.com/gp/book/9783642375200)).
* Python scripts to time and model-count Clafer, Z3 and DIMACS models.
* Python script to, making use of Z3 alternative functionality, transform numerical features modeled in Z3 into Tseitin-CNF DIMACS format using Bit Blasting. **It supports:**
  * First order logic features and operations (*AND*, *OR*, *XOR*...).
  * Booleans and signed and unsigned integers.
  * Arithmetic operations as: equality (*=*), inequalities (*<>*, *>*, *≥*,...), addition (*+*), etc.
  * Composed or multiple functions (e.g., *(a + b) > c*)
* Python script to time and obtain a certain number of random solutions (random sampling).  
* Python script to rank, estimate, and compare both ranks of a set of random solutions. Each set of N solutions is measured and obtained from a different reasoner.
* Intermediate files as set of samples, ranks, comparisons, solutions, etc.
* Uniform data distribution test Python scripts.
* more ([more](https://)).

## Usage

How to proceed to replicate the state of practice and/or add new tools:

* 

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
