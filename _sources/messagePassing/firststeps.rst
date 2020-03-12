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