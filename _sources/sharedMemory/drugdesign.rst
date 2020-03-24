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

The image of the above Gamma distribution of lengths of ligands came from: `here <https://keisan.casio.com/exec/system/1180573216>`_, where we used a = 4.2 and b = 0.8.

The Serial Version
^^^^^^^^^^^^^^^^^^^

The serial version of the code is accessible at this link: `dd_serial.cpp <http://selkie.macalester.edu/csinparallel/modules/DrugDesignInParallel/build/html/_downloads/dd_serial2.cpp>`_. 

Compile the code locally on your machine using the following command:

.. code-block:: bash

   g++ -o -o dd_serial dd_serial.cpp

To run the serial version of the program on your machine, run the following command:

.. code-block:: bash

   time -p ./dd_serial

The Parallel Versions
^^^^^^^^^^^^^^^^^^^^^

There are two parallel versions of the code available:

* A version that uses *static scheduling* and is implemented in OpenMP (``drugdesign_static``)

* A version that uses *dynamic scheduling* and is implemented in OpenMP (``drugdesign_dynamic``)

We will discuss static vs dynamic scheduling in greater detail later. For now, let's compile the two version and run them:

.. code-block:: bash

   g++ -o -o drugdesign-static drugdesign-static.cpp -fopenmp
   g++ -o -o drugdesign-dynamic drugdesign-dynamic.cpp -fopenmp 


You can measure the run-time of a particular iteration using ``time -p`` and specifying a number of threads. For example, 
to run the static version of the drug design implementation on 2 threads use the command:

.. code-block:: bash

   time -p ./drugdesign-static 2

**Exercise 1:**

Fill out the table by running the following series of tests:

.. tabularcolumns:: |l|l|l|l|l|

+--------------------------+---------+-----------+-----------+----------+
| Time (s)                 |1 Thread | 2 Threads | 3 Threads | 4 Threads|
+==========================+=========+===========+===========+==========+
| drugdesign-static        |         |           |           |          |
+--------------------------+---------+-----------+-----------+----------+
| drugdesign-dynamic       |         |           |           |          |
+--------------------------+---------+-----------+-----------+----------+


**Exercise 2:**

(interactive question: which version is faster?)


**Exercise 3:**

Recall that the equation for speedup is:

.. math::

    S_n = \frac{T_1}{T_n}

Where :math:`T_1` is the time it takes to execute a program on one thread, :math:`T_n` is the time it takes to execute that same program on *n* threads, and :math:`S_n` is the associated speedup.

We will use Python to assist us with our speedup calculation. Fill in the code below to compute the speedup for each version on each set of threads:

.. activecode:: dd_speedup
   :language: Python
   :caption: Calculate Speedup

   #lists holding measured times (floating point)
   #TODO: Fill in arrays below (code will not compile otherwise!)
   #            1 2 3 4
   dd_static = [ , , , ]
   dd_dynamic= [ , , , ]
   
   #compute speedup
   static_speedup  = [round(dd_static[0]/dd_static[i],2)   for i in range(1,4)]
   dynamic_speedup = [round(dd_dynamic[0]/dd_dynamic[i],2) for i in range(1,4)]

   print("static speedup:")
   print(static_speedup)

   print("dynamic speedup:")
   print(dynamic_speedup)


Static vs. Dynamic Scheduling
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Should point out that the two programs that students have played with thus far the work was statically scheduled. 

Talk about why in the drug design example, dynamic scheduling is better. 

