

CHAPTER 0: Introduction
:::::::::::::::::::::::::

0.1 What is Parallel Computing?
--------------------------------

This section contains a brief history of parallel computing, and its modern uses. Questions that we should be answering:

* what is it used for?
* why should students care?
* what can students expect to learn by the end of this book?


0.2 Important Concepts
------------------------

Before we continue, let's define some important concepts...

* core 
* multicore vs. distributed architectures
* task
* work
* concurrency vs. parallel
* shared memory vs. message passing
* speedup vs efficiency


0.3 Getting Access to a Parallel System
----------------------------------------


Runestone uses the ``restructuredText`` (rst) markup language.  We chose this over markdown largely because rst is extensible.  Nearly all of the basic markup tasks are already handled by restructuredText.  You should check out the docs for the basics of restructuredText (link below). Our extensions are all for the interactive elements.  One key hint about restructuredText:  Its like **Python** -- *indentation matters!*

* `restructuredText Docs <http://docutils.sourceforge.net/rst.html>`_
* `Runestone Docs <https://runestone.academy/runestone/static/authorguide/index.html>`_
* Join the discussion on our `Google Group <https://groups.google.com/forum/#!forum/runestone_instructors>`_
* Tell us about problems on `Github <https://github.com/RunestoneInteractive/RunestoneComponents>`_



Sample Directives
****************************

Some code examples below

ActiveCode
**********

.. activecode:: codeexample1
   :coach:
   :caption: This is a caption

   print("My first program adds a list of numbers")
   myList = [2, 4, 6, 8, 10]
   total = 0
   for num in myList:
       total = total + num
   print(total)

Multiple Choice
********************

.. mchoice:: question1_2
    :multiple_answers:
    :correct: a,b,d
    :answer_a: red
    :answer_b: yellow
    :answer_c: black
    :answer_d: green
    :feedback_a: Red is a definitely on of the colors.
    :feedback_b: Yes, yellow is correct.
    :feedback_c: Remember the acronym...ROY G BIV.  B stands for blue.
    :feedback_d: Yes, green is one of the colors.

    Which colors might be found in a rainbow? (choose all that are correct)

These are just two of the many interactive components for writing online course materials.  You can see examples of all of them `On our Example Page <http://interactivepython.org/runestone/static/overview/overview.html>`_

Now feel free to modify this file to start creating your own interactive page.


Section 4: Theme
*****************

You can override the style rules in the default theme by adding css rules to a file named **theme-overrides.css** (the filename is important - this will replace an existing file). Make sure the file's directory is part of the ``html_static_path``. You can do so by placing it in a folder **_static**, then modifying ``html_static_path`` in conf.py to include that folder:

.. code:: 

    html_static_path =  runestone_static_dirs() + ['_static']


If you want to do more significant changes to the theme, you should copy the files in the runestone/common/project/template/sphinx_bootstrap to a directory like ``_templates/my_theme``. Then make sure these values are set in conf.py:

.. code:: 

    html_theme_path = ["_templates"]
    html_theme = 'my_theme'