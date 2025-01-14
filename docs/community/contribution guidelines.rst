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

Learn how to build Synfig here - https://synfig-docs-dev.readthedocs.io/en/latest/common/building.html

Make sure to read about code structure - https://synfig-docs-dev.readthedocs.io/en/latest/common/structure.html

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

Thank You
~~~~~~~~~

If you follow these guidelines closely your contribution will have a
very positive impact on the Synfig project.

Thanks a lot for your patience.
