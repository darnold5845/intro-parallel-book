1.3 Reduction
---------------------------------

Fixing the integration example using the ``omp critical`` section is quite complex. Instead, we will show you a simpler way to do it, by
employing the concept of **reduction**. The notion of a reduction comes from the mathematical operation *reduce*, in which a collection of 
values are combined into a single value via a common mathematical function. Summing up a collection of values is therefore a natural 




Explain what a reduction is and how that is an alternative.

Introduce the reduction patternlet. 

Ask students to modify their programs to use reduction.

Have them re-time their programs on multiple threads. Is the program faster with or without reduction?