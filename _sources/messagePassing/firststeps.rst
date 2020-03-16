2.1 First Steps - Programming with Message Passing
---------------------------------------------------

Introduce MPI as a standard for message passing. Start with MPI patternlets for SPMD and master worker. 

The SPMD Pattern
^^^^^^^^^^^^^^^^
Here is the heart of this program that illustrates the concept of a single program that uses multiple processes, each containing and producing its own small bit of data (in this case printing something about itself).

.. literalinclude:: code/mpi4py/00spmd.py
  :language: python
  :lines: 23-

Let's look at each line in main() and the variables used.

1. *comm* The fundamental notion with this type of computing is a *process* running independently on the computer. With one single program like this, we can specify that we want to start several processes, each of which can **communicate**. The mechanism for communication is initialized when the program starts up, and the object that represents the means of using communication between processes is called MPI.COMM_WORLD, which we place in the variable comm.

2. *id* Every process can identify itself with a number. We get that number by asking *comm* for it using Get_rank().

3. *numProcesses* It is helpful to know haw many processes have started up, because this can be specified differently every time you run this type of program. Asking *comm* for it is done with Get_size().

4. *myHostName* When you run this code on a cluster of computers, it is sometimes useful to know which computer is running a certain piece of code. A particular computer is often called a 'host', which is why we call this variable myHostName, and get it by asking *comm* to provide it with Get_processor_name().

These four variables are often used in every MPI program. The first three are often needed for writing correct programs, and the fourth one is often used for debugging and analysis of where certain computations are running.

The fundamental idea of message passing programs can be illustrated like this:

.. image:: images/comm_world.png

Each process is set up within a communication network to be able to communicate with every other process via communication links. Each process is set up to have its own number, or id, which starts at 0.

.. note:: Each process holds its own copies of the above 4 data variables. **So even though there is one single program, it is running multiple times in separate processes, each holding its own data values.** This is the reason for the name of the pattern this code represents: single program, multiple data. The print line at the end of main() represents the multiple different data output being produced by each process.

Introduce Send and Receive and show a naieve implementation of integration using MPI. Show how 
the send and receive can be used to send an array of values and calculate the sum. 

May also want to talk about dealing with race conditions. 

The Master-Worker Pattern
^^^^^^^^^^^^^^^^^^^^^^^^^

**Program file:** 01masterWorker.py

**Example usage:**

    python run.py ./01masterWorker.py 4

Here the 4 signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercises:**

- Rerun, using varying numbers of processes from 1 through 8 (i.e., vary the last argument to run.py).
- Explain what stays the same and what changes as the number of processes changes.

Dive into the code
^^^^^^^^^^^^^^^^^^

What is different between this example and the previous one?

.. literalinclude:: code/mpi4py/01masterWorker.py
  :language: python
  :lines: 23-

The answer to the above question illustrates what we can do with this pattern: based on the process id, we can have one process carry out something different than the others. This concept is used a lot as a means to coordinate activities, where one process, often called the master, has the responsibility of handing out work and keeping track of results. We will see this in later examples.

.. note:: By convention, the master coordinating process is usually the process number 0.