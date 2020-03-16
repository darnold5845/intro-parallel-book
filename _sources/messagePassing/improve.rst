2.3 Other Communication Patterns
---------------------------------

There are many cases when a master process obtains or creates data that needs to be sent or 
received from all the other processes. In this section, we will discuss some special 
communication constructs specifically for those purposes.

Broadcast
^^^^^^^^^

Broadcast a dictionary
~~~~~~~~~~~~~~~~~~~~~~

**Program file:** 08broadcast.py

**Example usage:**

  python run.py ./08broadcast.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run, using N = from 1 through 8 processes.

Dive into the code
++++++++++++++++++

Find the place in this code where the data is being broadcast to all of the processes. Match the prints to the output you observe when you run it.

.. literalinclude:: code/mpi4py/08broadcast.py
  :language: python
  :lines: 23-

Broadcast user input
~~~~~~~~~~~~~~~~~~~~~~

**Program file:** 09broadcastUserInput.py

**Example usage:**

python run.py ./09broadcastUserInput.py N dataString

Here the N signifies the number of processes to start up in mpi, which must be greater than one. The dataString must be supplied and represents the string that will be broadcast from the master process to the workers.

run.py executes this program within mpirun using the number of processes given and the dataString argument.

For example, in this special instance, you can send a string with spaces and other special characters it it in it like this:

 python run.py ./09broadcastUserInput.py 2 "hello\ world\!"

 .. warning:: This program is unlike any of the others and takes in a second argument, as shown above.

**Exercise:**

- Run, using N = from 1 through 8 processes, with a string of your choosing.

Dive into the code
++++++++++++++++++

Find the place in this code where the data is being broadcast to all of the processes. Match the prints to the output you observe when you run it.

.. literalinclude:: code/mpi4py/09broadcastUserInput.py
  :language: python
  :lines: 29-


Broadcast a list
~~~~~~~~~~~~~~~~~~~~~~

This just shows that other data structures can also be broadcast.

**Program file:** 11broadcastList.py

**Example usage:**

  python run.py ./11broadcastList.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run, using N = from 1 through 8 processes.

Dive into the code
++++++++++++++++++



.. literalinclude:: code/mpi4py/11broadcastList.py
  :language: python
  :lines: 24-



Scatter and Gather
^^^^^^^^^^^^^^^^^^

There are often cases when each process can work on some portion of a larger data structure. This can be carried out by having the master process maintain the larger structure and send parts to each of the worker processes, keeping part of the structure on the master. Each process then works on their portion of the data, and then the master can get the completed portions back.

This is so common in message passing parallel processing that there are two special collective communication functions called scatter and gather that handle this.


The mpi4py library of functions has several collective communication functions that are designed to work with arrays created using the python library for numerical analysis computations called *numpy*.

If you are unfamiliar with using numpy, and want to know more about its features and available methods, you will need to consult another tutorial for that. It should be possible to understand the following scatter, then gather example by observing the results that get printed, even if you are unfamiliar with the functions from numpy that are used to create the 1-D array.

The numpy library has special data structures called arrays, that are common in other programming languages. A 1-dimensional array of integers can be envisioned very much like a list of integers, where each value in the array is at a particular index. The mpi4py Scatter function, with a capital S, can be used to send portions of a larger array on the master to the workers, like this:

.. image:: images/Scatter_array.png

|

The result of doing this then looks like this, where each process has a portion of the original that they can then work on:

.. image:: images/after_Scatter_array.png

|

The reverse of this process can be done using the Gather function.

In this example, a 1-D array is created by the master, then scattered, using Scatter (capital S). After each smaller array used by each process is changed, the Gather (capital G) function brings the full array with the changes back into the master.

**Program file:** 16ScatterGather.py

**Example usage:**

  python run.py ./16ScatterGather.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercises:**

- Run, using N = from 2 through 8 processes.
- If you want to study the numpy part of the code, look up the numpy function linspace used in genArray().

Dive into the code
~~~~~~~~~~~~~~~~~~~

.. note:: In the code below, note how all processes must call the Scatter and Gather functions.

.. literalinclude:: code/mpi4py/16ScatterGather.py
  :language: python
  :lines: 24-



Returning to the Array Example
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Update the array addition example using scatter() method and 
introduce the gather() and reduce() functions.

Have students choose which one makes more sense gather() or reduce().

Then have them modify the program with reduce() and gather().

Have them time the performance of the two implementations.



Reduction
^^^^^^^^^

There are often cases when every process needs to complete a partial result of an overall computation. For example if you want to process a large set of numbers by summing them together into one value (i.e. *reduce* a set of numbers into one value, its sum), you could do this faster by having each process compute a partial sum, then have all the processes communicate to add each of their partial sums together.

This is so common in parallel processing that there is a special collective communication function called **reduce** that does just this.

The type of reduction of many values down to one can be done with different types of operators on the set of values computed by each process.

Reduce all values using sum and max
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this example, every process computes the square of (id+1). Then all those values are summed together and also the maximum function is applied.

**Program file:** 12reduction.py

**Example usage:**

  python run.py ./12reduction.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercises:**

- Run, using N = from 1 through 8 processes.
- Try replacing MPI.MAX with MPI.MIN(minimum) and/or replacing MPI.SUM with MPI.PROD (product). Then save and run the code again.

Dive into the code
++++++++++++++++++

Find the place in this code where the data computed on each process is being reduced to one value. Match the prints to the output you observe when you run it.

.. literalinclude:: code/mpi4py/12reduction.py
  :language: python
  :lines: 23-

Reduction on a list of values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We can try reduction with lists of values, but the behavior matches Python semantics regarding lists.

**Program file:** 13reductionList.py

**Example usage:**

  python run.py ./13reductionList.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercises:**

- Run, using N = from 1 through 4 processes.
- Uncomment the two lines of runnable code that are commented in the main() function. Observe the new results and explain why the MPI.SUM (using the + operator underneath) behaves the way it does on lists, and what the new function called sumListByElements is doing instead.

Dive into the code
++++++++++++++++++

In this code, try to explain what the function called sumListByElements does. If you are unfamiliar with the zip function, look up what it does.

.. literalinclude:: code/mpi4py/13reductionList.py
  :language: python
  :lines: 27-

.. note:: There are two ways in Python that you might want to sum a set of lists from each process: 1) concatenating the elements together, or 2) summing the element at each location from each process and placing the sum in that location in a new list. In the latter case, the new list is the same length as the original lists on each process.