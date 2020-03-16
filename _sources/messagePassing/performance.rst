2.4 Performance Considerations: Deadlock
-----------------------------------------

A common source of errors in programs that use point-to-point communication is called **deadlock**. In this section, we define what deadlock is, and how 
to fix it.

Deadlock
^^^^^^^^
The following code represents a common error that many programmers have inadvertently placed in their code. The concept behind this program is that we wish to use communication between pairs of processes, like this:

.. image:: images/pair_exchange.png

For message passing to work between a pair of processes, one must send and the other must receive. If we wish to **exchange** data, then each process will need to perform both a send and a receive.
The idea is that process 0 will send data to process 1, who will receive it from process 0. Process 1 will also send some data to process 0, who will receive it from process 1. Similarly, processes 2 and 3 will exchange messages: process 2 will send data to process 3, who will receive it from process 2. Process 3 will also send some data to process 2, who will receive it from process 3.

If we have more processes, we still want to pair up processes together to exchange messages. The mechanism for doing this is to know your process id. If your id is odd (1, 3 in the above diagram), you will send and receive from your neighbor whose id is id - 1. If your id is even (0, 2), you will send and receive from your neighbor whose id is id + 1. This should work even if we add more than 4 processes, as long as the number of processes is divisible by 2.

.. warning:: There is a problem with the following code called *deadlock*. This happens when every process is waiting on an action from another process. The program cannot complete. On linux systems such as the Raspberry Pi, type ctrl-c together to stop the program (ctrl means the control key).


**Program file:** 04messagePassingDeadlock.py

**Example usage:**

  python run.py ./04messagePassingDeadlock.py 4

Here the 4 signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run this code with 4 processes. Observe the deadlock and stop the program using ctrl-c.


Dive into the code
++++++++++++++++++

In this code, can you trace what is happening to cause the deadlock?

.. reveal:: reveal-deadlock1
   :modal:
   :modaltitle: Understanding deadlock
   :showtitle: Reveal Answer
   :hidetitle: Hide Answer

   Each process, regardless of its id, will execute a receive request first. In this model, recv is a **blocking** function- it will not continue until it gets data from a send. So every process is blocked waiting to receive a message.

.. literalinclude:: code/mpi4py/04messagePassingDeadlock.py
  :language: python
  :lines: 21-

Can you think of how to fix this problem?

.. reveal:: reveal-deadlock-fix1
   :modal:
   :modaltitle: Fixing deadlock
   :showtitle: Reveal Answer
   :hidetitle: Hide Answer

   Since recv is a **blocking** function, we need to have some processes send first, while others correspondingly recv first from those who send first. This provides coordinated exchanges.


Visualizing Deadlock
+++++++++++++++++++++

Put TSGL visualization of deadlock here.

Discuss the dining philosophers problem here, and show TSGL visualization.



Fixing Deadlock
^^^^^^^^^^^^^^^
To fix deadlock of the previous example, we coordinate the communication between pairs of processes so that there is an ordering of sends and receives between them.

.. note:: The new code corrects deadlock with a simple change: odd process sends first, even process receives first. *This is the proper pattern for exchanging data between pairs of processes.*

.. literalinclude:: code/mpi4py/05messagePassing.py
  :language: python
  :lines: 26-

**Program file:** 05messagePassing.py

**Example usage:**

  python run.py ./05messagePassing.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run, using N = 4, 6, 8, and 10 processes. (Note what happens if you use an odd number.)

Sending data structures
+++++++++++++++++++++++

This next example illustrates that we can exchange different lists of data between processes.

**Program file:** 06messagePassing2.py

**Example usage:**

  python run.py ./06messagePassing2.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run, using N = 4, 6, 8, and 10 processes.

Dive into the code
++++++++++++++++++

In the following code, locate where the list of elements to be sent is being made by each process.

.. literalinclude:: code/mpi4py/06messagePassing2.py
  :language: python
  :lines: 26-

Ring of passed messages
^^^^^^^^^^^^^^^^^^^^^^^

Another pattern that appears in message passing programs is to use a ring of processes: messages get sent in this fashion:

.. image:: images/ring.png


When we have 4 processes, the idea is that process 0 will send data to process 1, who will receive it from process 0 and then send it to process 2, who will receive it from process 1 and then send it to process 3, who will receive it from process 2 and then send it back around to process 0.

**Program file:** 07messagePassing5.py

**Example usage:**

  python run.py ./07messagePassing3.py N

Here the N signifies the number of processes to start up in mpi.

run.py executes this program within mpirun using the number of processes given.

**Exercise:**

- Run, using N = from 1 through 8 processes.

Dive into the code
+++++++++++++++++++

Compare the results from running the example to the code below. Make sure that you can trace how the code generates the output that you see.

.. literalinclude:: code/mpi4py/07messagePassing3.py
  :language: python
  :lines: 22-

.. mchoice:: mc-mp-ring
   :answer_A: The last process with the highest id will have 0 as its destination because of the modulo (%) by the number of processes.
   :answer_B: The last process sends to process 0 by default.
   :answer_C: A destination cannot be higher than the highest process.
   :correct: a
   :feedback_A: Correct! Note that you must code this yourself.
   :feedback_B: Processes can send to any other process, including the highest numbered one.
   :feedback_C: This is technically true, but it is important to see how the code ensures this.

   How is the finishing of the 'ring' completed, where the last process determines that it should send back to process 0?