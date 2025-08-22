Contributing guidelines
=======================

Hi there! Interested in contributing to Synfig? We’d love your help. Synfig is an open source project, built one contribution at a time by users like you. 
Please read these guidelines carefully, it will help you and us to save precious time later.

As you prepare to join our community, you should find out what this project is about - if you didn’t do this already. We highly recommend to become familiar with existing Synfig functionality before you start any coding. You can start with `beginner's tutorials <https://wiki.synfig.org/Category:Tutorials>`_. Also, you can request a free access to `our video training course <https://www.udemy.com/synfig-studio-cutout-animation-en/>`_, which allow you to grab all basics in the shortest time. 

.. note::

   - Download Synfig -> https://www.synfig.org/download-stable/
   - Join Synfig Forum -> https://forums.synfig.org/
   - We use GitHub to manage our repository. If you’re not familiar with git/GitHub, we strongly recommend following a tutorial, such as `this one <http://try.github.io/>`_ .    

Ways to contribute
~~~~~~~~~~~~~~~~~~

Whether you’re a developer, a designer, or just a Synfig devotee, there are lots of ways to contribute. Here’s a few ideas:

* Install Synfig on your computer and kick the tires. Does it work? Does it do what you’d expect? If not, open an issue and let us know.
* Comment on some of the project’s open issues. Have you experienced the same problem? Know a work around? Do you have a suggestion for how the feature could be better?
* Read through the documentation, and ask the community(on forum or GitHub), any time you see something confusing, or have a suggestion for something that could be improved.
* Browse through the Synfig discussion forum, and lend a hand answering questions. There’s a good chance you’ve already experienced what another user is experiencing.
* Find an open issue (especially those labeled help wanted/good first issue), mention yourself in the comment with the interest to work on the issue and submit a proposed fix. If it’s your first pull request, we promise we won’t bite, and are glad to answer any questions.
* Help evaluate open pull requests, by testing the changes locally and reviewing what’s proposed.
* Open an issue if you found something that needs a fix (**Please don't forget to mentioned if you want to work on the issue you opened or have started working for a fix!**)

.. note::

   - Synfig issue tracker -> https://github.com/synfig/synfig/issues
   - `Some beginner issue to ensure you can use synfig. <https://github.com/synfig/synfig-tests-regressions/issues/3>`_. 
   - Want easy projects? `Check out our list of "good first issues" <https://github.com/synfig/synfig/labels/good%20first%20issue>`_ .
   
Contributing code changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Get code and prepare your working tree
-----------------------------------------

.. warning::
    - Before you start working on any issue kindly ensure that someone else is not already working on it by looking at comments. 
    - If no one is working on that issue drop a comment to let everyone know you will be working on it.
    - We know your time is precious :)

Fork the project by clicking “Fork” in the top right corner of synfig/synfig.
Clone the repository locally 
::

  git clone https://github.com/<your-username>/synfig

Create a new, descriptively named branch to contain your change
::

  git checkout -b my-awesome-feature

2. Making code changes
-----------------------------------------

Learn how to :ref:`build Synfig <Building Synfig>`

Make sure to read about :ref:`code structure <structure>`

Don't forget to configure IDE. We recommend to use NetBeans. Here are video instructions on how to configure it - https://www.youtube.com/watch?v=SNkdiSxBV_s

Now, have fun, hack away.

- make sure to follow the code style used in the module
  you are contributing to
- before committing and pushing the changes, test the code both manually
  and automatically with the automated test suite if applicable

3. Commit message style guidelines
----------------------------------

When done, commit your changes.

- commits should be descriptive in nature
- the message should explain the nature of the change


4. Submitting a pull request
----------------------------

* The smaller the proposed change, the better. If you’d like to propose two unrelated changes, submit two pull requests.
* The more information, the better. Make judicious use of the pull request body. Describe what changes were made, why you made them, and what impact they will have for users.
* If this is your first pull request, it may help to understand GitHub Flow.

Push the branch (you have created it) to your GitHub fork: 
::

  git push origin my-awesome-feature

Create a pull request (from now on we will shorten it often to just *PR*) by visiting https://github.com/<your-username>/synfig and following the instructions at the top of the screen:

- from your forked repository of the project select your branch and
  click "New Pull Request"
- check the changes tab and review the changes again to ensure everything
  is correct
- write a concise description of the PR, if an issue exists for

.. note::
   While creating the PR please mention the issue number. For example, to close an issue numbered 123, you could use the phrase "Closes #123" or "Closes: #123" in your pull request description or commit message. Once the branch is merged into the default branch, the issue will close.

- after submitting your PR, check back again whether your PR has passed
  our required tests
- if the tests fail for some reason, try to fix them and if you get
  stuck ask for help.
- if the tests pass, maintainers will review the PR and may ask
  you to improve details or changes, please be patient: creating a good
  quality open source project takes a bit of sweat and effort; ensure
  to follow up with this type of operations
- once everything is fine with us we'll merge your PR

5. Avoiding unnecessary changes
-------------------------------

- while making changes to the required files, then saving it and
  comitting it, different contributors often find that there occur same
  changes that they have not made and those changes gets committed with
  the desired change that the person wants to make
- these unnecessary changes should be evaluated first before the
  commit should be made
- these changes generally occur due to different settings and
  customizations of your editor that you are working with. These changes
  are produced on their own as soon as you save a file. Examples are -
  Introducing new lines, removing and adding spaces, etc
- to avoid such changes please check your editor settings first. If this
  sort of behaviour persists please use any command line editor like
  VIM, etc

Coding style
~~~~~~~~~~~~

There are many creative ways a coder expresses his/her clever/intelligent solution
but we also believe uniformity is a necessary evil for both the contributors and
the maintainers. It boosts up code readability, encourages clean coding, and helps
both the readers (us) and the writer (you) during code review.

Below are some of the best practices to follow through while making code changes
to the Synfig project.

Namespace spacing
-----------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 4-6               |   :emphasize-lines: 4-6                  |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   // Indent class on the same line    |   // Do not indent class like below      |
|   namespace synfig {                  |   namespace synfig {                     |
|                                       |                                          |
|   class BLinePoint : public UniqueID  |       class BLinePoint : public UniqueID |
|   {                                   |       {                                  |
|   };                                  |       };                                 |
|                                       |                                          |
|   }                                   |   }                                      |
|                                       |                                          |
+---------------------------------------+------------------------------------------+

Class access modifier spacings
------------------------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 3, 6, 9           |   :emphasize-lines: 3, 6, 9              |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   class BLinePoint : public UniqueID  |   class BLinePoint : public UniqueID     |
|   {                                   |   {                                      |
|       public:                         |   public:                                |
|           ...                         |       ...                                |
|                                       |                                          |
|       protected:                      |   protected:                             |
|           ...                         |       ...                                |
|                                       |                                          |
|       private:                        |   private:                               |
|           ...                         |       ...                                |
|   };                                  |   };                                     |
|                                       |                                          |
+---------------------------------------+------------------------------------------+

Class initializer list spacings
-------------------------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 2, 3, 4           |   :emphasize-lines: 1, 2                 |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   BLinePoint::BLinePoint()            |   BLinePoint::BLinePoint() : width_(0),  |
|       : vertex_ (Point(0, 0))         |       origin_(0.0), vertex_(Point(0, 0)) |
|       , width_  (0)                   |   {                                      |
|       , origin_ (0.0)                 |                                          |
|   {                                   |       ...                                |
|       ...                             |                                          |
|   }                                   |   }                                      |
|                                       |                                          |
+---------------------------------------+------------------------------------------+

Class methods return type spacing
---------------------------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 1, 7              |   :emphasize-lines: 1, 8                 |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   void                                |   void synfig::BLinePoint::reverse()     |
|   synfig::BLinePoint::reverse()       |   {                                      |
|   {                                   |       ...                                |
|       ...                             |   }                                      |
|   }                                   |                                          |
|                                       |                                          |
|   synfig::GUID                        |                                          |
|   Activepoint::get_guid()             |   synfig::GUID Activepoint::get_guid()   |
|   {                                   |   {                                      |
|       ...                             |       ...                                |
|   }                                   |   }                                      |
|                                       |                                          |
+---------------------------------------+------------------------------------------+

Specifiers & modifiers spacing
------------------------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 1, 7              |   :emphasize-lines: 1, 7                 |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   inline void                         |   inline                                 |
|   clamp(Vector& v)                    |   void clamp(Vector& v)                  |
|   {                                   |   {                                      |
|       ...                             |       ...                                |
|   }                                   |   }                                      |
|                                       |                                          |
|   static inline unsigned char*        |   static inline                          |
|   color2pf_raw()                      |   unsigned char* color2pf_raw()          |
|   {                                   |   {                                      |
|       ...                             |       ...                                |
|   }                                   |   }                                      |
|                                       |                                          |
+---------------------------------------+------------------------------------------+


Operator spacings
-----------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|                                       |                                          |
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 6                 |   :linenos:                              |
|   :linenos:                           |                                          |
|                                       |                                          |
|   // Unary                            |   // Unary                               |
|   i++;                                |   i ++;                                  |
|   if (!b) {}                          |   if (! b) {}                            |
|                                       |                                          |
|   // Assignment, binary, ternary      |   // Assignment, binary, ternary         |
|   y = m*x + b;                        |   y = m*x+b;                             |
|   c = a | b;                          |   c=a|b;                                 |
|   return condition ? true : false;    |   return condition?true:false;           |
|                                       |                                          |
+---------------------------------------+------------------------------------------+
| | To emphasize order no spacing is OK |                                          |
|   sometimes                           |                                          |
| | (line no 6)                         |                                          |
+---------------------------------------+------------------------------------------+

Braces & more spaces
--------------------

.. code-block:: c++

   /**
    * All C++ reserved keywords where braces are inevitable
    * Are placed on the same line
    *
    * Also make sure to leave one space before function brackets
    */

   if (condition) {
       ...
   } else {
       ...
   }

   switch (value) {
       case 1: ...
       case n: ...
   }

   enum MyEnum {
       ...
   };
   
   /* Same apply for C++ loops too */

   while (condition) {
       ...
   }

   do {
       ...
   } while (condition);

   for (const auto& item : list) {
       ...
   }
   
   // Excepting lambda expressions; do not space the function bracket
   [](int a, int b) { ... }

   // And user defined functions:
   // - Brace on the new line
   // - And no space before function bracket
    
   void
   my_function()
   {
       another_function();
   }
   
   // including main too
   int
   main(int argc, char** argv)
   {
       return 0;
   }

Single line control statement
-----------------------------

.. code-block:: c++
   :linenos:
   
   // They are acceptable only if the body has a single instruction/statement

   while (reading) { i++; }
   if (!ready) return;
   for (;;) { do_something(); ++i; }

Naming & cases
---------------

+-------------------------+------------+
| Naming                  | Cases      |
+============+============+============+
| class      | type       | CamelCase  |
|            +------------+------------+
|            | properties |            |
|            +------------+ snake_case |
|            | methods    |            |
+------------+------------+------------+
|            | type       | CamelCase  |
| enum       +------------+------------+
|            | values     | UPPERCASE  |
+------------+------------+------------+
| structs                 | CamelCase  |
+-------------------------+------------+
| variables               | snake_case |
+-------------------------+------------+
| functions               | snake_case |
+-------------------------+------------+
| constants               | UPPERCASE  |
+-------------------------+------------+
| macros                  | UPPERCASE  |
+-------------------------+------------+

Pointers & references
---------------------

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   int* snake_case;                    |   int *snake_case;                       |
|                                       |                                          |
|   const int& UPPERCASE;               |   const int & UPPERCASE;                 |
+---------------------------------------+------------------------------------------+
| Do not add space between the data type and the pointer or reference              |
+----------------------------------------------------------------------------------+

Code documenting
----------------

.. code-block:: c++
   :linenos:

    /**
     * Use JavaDoc style
     * to document your complex ideas so others in the future can read,
     * comprehend, and hack/improve your code :)
     *
     * And to document your parameters & return values use:
     * @param foo ...
     * @return bar ...
     */

Headers & macros
--------------------

Indentation
^^^^^^^^^^^

+---------------------------------------+------------------------------------------+
| Correct                               | Incorrect                                |
+=======================================+==========================================+
|.. code-block:: c++                    |.. code-block:: c++                       |
|   :emphasize-lines: 2, 5              |   :emphasize-lines: 2, 5                 |
|   :linenos:                           |   :linenos:                              |
|                                       |                                          |
|   #ifdef USING_PCH                    |   #ifdef USING_PCH                       |
|   # include "pch.h"                   |   #include "pch.h"                       |
|   #else                               |   #else                                  |
|   #ifdef HAVE_CONFIG_H                |   #ifdef HAVE_CONFIG_H                   |
|   # include <config.h>                |   #include <config.h>                    |
|   #endif                              |   #endif                                 |
|   #endif                              |   #endif                                 |
+---------------------------------------+------------------------------------------+

Legacy include guard
^^^^^^^^^^^^^^^^^^^^^^

Synfig is somewhat ancient so you might encounter legacy coding style like below:

.. code-block:: c++

   #define __SYNFIG_ROTATE_H

The double underscore before defining the header file is reserved for use by the
C++ standard. So if you are designing a new class be sure to not prefix your header
file macro guard with the double underscores.

GUI class identifier
^^^^^^^^^^^^^^^^^^^^

When creating a new GUI class, we recommend the identifier format for your header
(replace XXXXX) file:

.. code-block:: c++

   #define SYNFIG_STUDIO_XXXXX_H

Include order
^^^^^^^^^^^^^

Include headers in the following order: Related header, C system headers, C++
standard library headers, other libraries' headers, and finally your project
headers.

.. code-block::c++

   #

Thank You
~~~~~~~~~

If you follow these guidelines closely your contribution will have a
very positive impact on the Synfig project.

Thanks a lot for your patience.
