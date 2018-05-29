.. image:: https://travis-ci.org/srnas/barnaba.svg
    :target: https://travis-ci.org/srnas/barnaba
.. image:: https://badge.fury.io/py/barnaba.svg
    :target: https://badge.fury.io/py/barnaba
.. image:: https://repology.org/badge/version-for-repo/macports/python:barnaba.svg
    :target: https://repology.org/metapackage/python:barnaba
.. image:: https://codecov.io/gh/srnas/barnaba/branch/master/graph/badge.svg
    :target: https://codecov.io/gh/srnas/barnaba


Introduction
============

BaRNAba is a tool for analyzing RNA three-dimensional structures and simulations. BaRNAba uses MDtraj to read/write topology and trajectory files, as such it supports several formats including pdb, xtc, trr, dcd, binpos, netcdf, mdcrd, prmtop, and more.  
BaRNAba has been developed by Sandro Bottaro with the crucial help of Giovanni Bussi, Giovanni Pinamonti, Sabine Reisser and Wouter Boomsma.   

This is what you can do with baRNAba:  

1. Calculate eRMSD [1]
2. Calculate RMSD after optimal alignment  
3. Search for single/double stranded RNA motifs in the PDB database or in simulations [1]  
4. Annotate PDB structures and trajectories with the Leontis-Westhof classification
5. Cluster nucleic acids structures using the eRMSD as a metric distance
6. Calculate elastic network models for nucleic acids and nucleic acids/protein complexes [2]
7. Calculate backbone and pucker torsion angles in a PDB structure or trajectory
8. Back-calculate 3J scalar couplings from PDB structure or trajectory
9. Score three-dimensional structures using eSCORE [1]

For bugs, questions or comments contact Sandro at sandro dot bottaro (guesswhat) gmail dot com

If you use baRNAba in your work,  please cite the following paper::

      @article{bottaro2014role,   
               title={The role of nucleobase interactions in RNA structure and dynamics},  
               author={Bottaro, Sandro and Di Palma, Francesco and Bussi, Giovanni},  
               journal={Nucleic acids research},  
               volume={42},  
               number={21},  
               pages={13306--13314},  
               year={2014},  
               publisher={Oxford University Press}  
     }



Requirements
-------------
baRNAba requires:
   - Python 2.7.x or > 3.3
   - Numpy
   - Scipy
   - Mdtraj 1.9
   - future
     
baRNAba requires mdtraj (http://mdtraj.org/) for manipulating structures and trajectories. 
To perform cluster analysis, scikit-learn is required too.

Required packages can be installed using `pip`, e.g.:

    pip install mdtraj

Installation
-------------

You can obtain the latest tagged version of barnaba using pip:

    pip install barnaba

On MacOS, you can install the same tagged version using the python distributed with MacPorts:

    sudo port install py36-barnaba

Just replace 36 with the python version that you prefer to use.
  
Alternatively, you can find the most recent version of barnaba on Github:

    git clone git://github.com/srnas/barnaba.git

then move to the barnaba directory and run the command

    pip install -e .

Notice that barnaba is installed using `setuptools_scm` that is not compatible with git archives. Thus,
you would not be able to install barnaba from a git tarball.

Some users might experience issues installing MDtraj using pip. If this is the case, we recommend installing MDtraj using conda:

    conda install --channel omnia mdtraj
    
Usage
------------
BaRNAba can be either used as a Python library or as a commandline tool.
A number of Notebook examples can be found in the examples_ directory.

Alternatively, the command-line interface can be found in the bin directory. Here's a minimal how-to

0.  minimal help:
    barnaba --help  
  
1. Calculate the ERMSD between structures  

   barnaba ERMSD --ref ../test/data/sample1.pdb --pdb ../test/data/sample2.pdb
  
   trajectories can be provided as well, by specifying a topology file  

   barnaba ERMSD --ref ../test/data/sample1.pdb --top ../test/data/sample1.pdb --trj ../test/data/samples.xtc  

   other accepted options are shown in a function-specific help  

   barnaba ERMSD --help
  
2. Calculate the RMSD between structures  
  
   barnaba RMSD --ref ../test/data/sample1.pdb --pdb ../test/data/sample2.pdb --dump
   
3. Find single stranded motif  
  
   barnaba SS_MOTIF --query ../test/data/GNRA.pdb --pdb ../test/data/1S72.pdb   
   
4. Find double stranded motif. l1 and l2 are the lengths of the two strands
  
   barnaba DS_MOTIF --query ../test/data/SARCIN.pdb --pdb ../test/data/1S72.pdb --l1 8 --l2 7  
 
5. Annotate structures/trajectories according to the Leontis/Westhof classification.
   
   barnaba ANNOTATE --pdb ../test/data/SARCIN.pdb  

6. Calculate backbone/sugar/pseudorotation angles
    
   barnaba TORSION --pdb ../test/data/GNRA.pdb --backbone --sugar --pucker 
 

7. Calculate J-couplings 

   barnaba JCOUPLING --pdb ../test/data/sample1.pdb 

8. Calculate elastic network models for RNA and predict SHAPE reactivity. NB: only works with PDB.
   
   barnaba ENM --pdb ../test/data/GNRA.pdb --shape

9. Calculate relative positions between bases R_ij  ang G vectors for pairs within ellipsoidal cutoff  

   barnaba DUMP --pdb ../test/data/GNRA.pdb --dumpG --dumpR  

10. Extract fragments from structures with a given sequence. NB: only works with PDB.  

    barnaba SNIPPET --pdb ../test/data/1S72.pdb  --seq NNGNRANN
 
11. Calculate ESCORE  
    
   barnaba ESCORE --ff ../test/data/1S72.pdb --pdb ../test/data/sample1.pdb


References
------------

[1] Bottaro, Sandro, Francesco Di Palma, and Giovanni Bussi.  
    "The role of nucleobase interactions in RNA structure and dynamics."  
    Nucleic acids research 42.21 (2014): 13306-13314.  

[2] Pinamonti, Giovanni, et al.  
   "Elastic network models for RNA: a comparative assessment with molecular dynamics and SHAPE experiments."  
   Nucleic acids research 43.15 (2015): 7260-7269.

.. _examples: https://github.com/srnas/barnaba/tree/master/examples
