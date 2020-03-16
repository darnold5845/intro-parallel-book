2.2 Point to Point Communication
--------------------------------

The fundamental basis of coordination between independent processes is point-to-point communication between processes through the communication links in the MPI.COMM_WORLD. The form of communication is called message passing, where one process **sends** data to another one, who in turn must **receive** it from the sender. This is illustrated as follows:

.. image:: images/send_recv.png

Point-to-point communication is a way that pair of processors transmits the data between one another, one processor sending, and the other receiving. MPI provides ``Send()`` and ``Receive()`` functions that allow point-to-point communication taking place in a communicator. For general Python objects, use the 
lowercase ``send()`` and ``receive()`` functions instead. ::

  Send(message, destination, tag)

    - message:  numpy array containing the message
    - destination:  rank of the receiving process
    - tag:    message tag is a way to identify the type of a message (optional)

  Recv(message, source, tag)
    - message: numpy array containing the message
    - source: rank of the sending process 
    - tag: message tag is a way to identify the type of a message

.. note:: The ``send()`` and ``recv()`` functions have identical form, except that the ``message`` parameter could be a standard Python type (e.g. int, string, float etc.). Be sure to use the uppercase form and ``numpy`` arrays whenever dealing with list-like data!


Send and Receive Hello World
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. literalinclude:: code/mpi4py/sendReceive.py
  :language: python


This MPI program illustrates the use of ``send()`` and ``recv()`` functions. Notice that we use the lowercase version of the Send/Receive functions due to the 
fact that the message being sent is of type ``String``. 

The above program can be run using the following command:

.. code-block:: bash

   python run.py ./sendReceive 4



Introduce Send and Receive and show a naieve implementation of integration using MPI. Show how 
the send and receive can be used to send an array of values and calculate the sum. 

May also want to talk about dealing with race conditions. 