1.5 Real World Problem - Drug Design
-------------------------------------

Let's look at a larger example. An important problem in the biological sciences is that of drug design. The goal is to find small molecules, called ligands, that are good candidates for use as drugs.

This is a very rough simulation of a program to compute how well a set of short protein *ligands* (each a possible drug) matches a given longer protein string. In the real software programs that do this, the matching is quite sophisticated, targeting possible 'receptor' sites on the protein.

Here is an image illustrating the concept of the ligand (represented by small sticks in center) binding to areas of the protein (represented by ribbon structure):

.. image:: images/proteinligand.jpg

For the real versions of this code and our simulated case, the longer the ligand or the longer the protein, the longer it takes for the matching and score of the match to complete.

We have created a default fake protein in the code. This can be changed on the command line.

We create the list of possible ligands in 2 ways:

1. If the number of ligands is <= 18, the list of ligands comes from a fabricated list that is hard-coded in the code. We designed this as an example with a range of ligands whose length was from 2 through 6.

2. If the number of ligands is > 18, the ligands are generated randomly using a gamma distribution that looks like this:

.. image:: images/gamma_dist.png

This means that we will have more ligands of length 2 or 3 and fewer of 4, 5, and 6, which are each declining in number. This has no relation to the real problem, but instead gives us some ligands that can be computed in a reasonable amount of time on a small cluster of single board computers.  This way you can try some experiments and be able to see the advantage of one implementation over the other.

The image of the above Gamma distribution of lengths of ligands came from: `here <https://keisan.casio.com/exec/system/1180573216>`_, where we used a = 4.2 an b = 0.8.

The Serial Version
^^^^^^^^^^^^^^^^^^^

The code is too long to run interactively. Instead, we encourage you to download the code examples and run them on your machine. 
To run the serial version of the program on your machine, run the following command:


The Parallel Versions
^^^^^^^^^^^^^^^^^^^^^

There are two parallel versions of the code that we make available to you...


(interactive question: which version is faster?)



Static vs. Dynamic Scheduling
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Should point out that the two programs that students have played with thus far the work was statically scheduled. 

Talk about why in the drug design example, dynamic scheduling is better. 

