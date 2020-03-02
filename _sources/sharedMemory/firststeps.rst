
1.1 First Steps - Programming Shared Memory
-------------------------------------------

In this book, we use the Open Multi-Processing (**OpenMP**) library to demonstrate how 
to program shared memory systems. While OpenMP is just one of many options for programming 
shared memory systems, we choose OpenMP due to its pervasiveness and relative ease of use. 
OpenMP employs a series of compiler directives to enable users to incrementally add 
parallelism to their programs and is natively supported by GCC. 

As a means of introducing OpenMP, we begin by introducing the **fork-join pattern** for parallel programming.
In fork-join, the main thread "forks" (or creates) a series of threads that all perform the same task in parallel.
When each thread terminates, their executions "join" (or merge) together back into the main thread.

Consider the following serial program. What is its output?

**Listing 3**

.. _lst_showmethod:

.. activecode:: showmethod
  :language: cpp
  :caption: Show method implementation

  //using functions to print fractions to the command line.
  #include <iostream>
  using namespace std;

  class Fraction {
      public:
          Fraction(int top = 0, int bottom = 1){
              num = top;
              den = bottom;
          }
          void show(){
              cout << num << "/" << den << endl;
          }
      private:
          int num, den;
  };

  int main() {
      Fraction fraca(3, 5);
      Fraction fracb(3);
      Fraction fracc; //notice there are no parentheses here.
      // cout << fraca << endl; //uncomment to see error
      fraca.show();
      fracb.show();
      fracc.show();
      return 0;
  }
  
Walk through fork-join patternlet and use output to motivate a discussion of thread execution and 
to define non-determinism.

Introduce the SPMD patternlet, and define what single program multiple data is, and how it relates 
to the notion of data parallelism. As an example, 
discuss the process of array addition (sample unplugged activity). 

Contrast "task parallelism" with "data parallelism" and mention how most parallel programs are 
somewhere along the spectrum. Recognize that both strategies are ways to assign work to threads.