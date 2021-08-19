For developers
===============

Definition of authors' comments
-------------------------------

The following syntax allow you to write a comment in color with your initials:

.. code-block:: html

    :red:`[This is a comment.]INITIALS`

And it will appear like this:

:red:`[This is a comment.]INITIALS`

The available colors are: :gray:`gray`, :silver:`silver`, :maroon:`maroon`, :red:`red`, :magenta:`magenta`, :fuchsia:`fuchsia`, :pink:`pink`, :orange:`orange`,
:yellow:`yellow`, :lime:`lime`, :green:`green`, :olive:`olive`, :teal:`teal`, :cyan:`cyan`, :aqua:`aqua`, :blue:`blue`, :navy:`navy`, :purple:`purple`.

You can see here the definition and the rendering of the current author's comments:

:red:`This is a comment from Bryan Convens.]BC`
    
:teal:`[This is a comment from Kelly Merckaert.]KM`
    
:maroon:`[This is a comment from Zakaria Lakeel.]ZL`
    
:lime:`[This is a comment from Titouan Tyack.]TT`
    
:blue:`[This is a comment from Jonathan Vogt.]JV`

How to use ReadTheDocs ?
------------------------

In this chapter, we will explain the basics of ReadTheDocs.

How to produce the ReadTheDocs website ?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create this tutorial, we used a documentation generator called Sphinx and reStructuredText. We refered to the `ReadTheDocs Documentation <https://docs.readthedocs.io/en/stable/index.html#>`__
and `ReStructuredText primer <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`__.

How to open the ReadTheDocs website ?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Just follow `these instructions <https://github.com/mrs-brubotics/documentation_brubotics>`__.

How to edit the ReadTheDocs website ?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It can be easier for you to code your website by using Visual Studio Code with an extension called reStructuredText wich is useful to previsualize your
website. It has a syntax highlighting tool.

The only files you need to modify are ``conf.py`` and all the ``.rst`` files in the ``source`` folder. Once you want to update your documentation, use the following
commands from your directory:

* Before all, we recommend you to run this command to check which files have been updated since the last commit:
  
.. code-block:: shell

    git status

* To update your local repository and get the newest code, run this command:
    
.. code-block:: shell    

    git pull

.. note::

    The pull can not work if you have made changes in the files without committing and if someone also made commits If so, run this command:

    .. code-block:: shell    

        git stash

* To update your repository, adapt and run these commands

.. code-block:: shell

    git add       # use tab key and type the first letter of the files to commit or use git add -A to directly stage all files
    git commit -m "Provide a clear explanation of your commit. People who did not make the change should understand the issue you solved."
    git push

Please refer to section :ref:`2.4 <2.4 Working with Git>` to understand why we use these commands.

.. note::
    When visualizing the documentation after running ``make html``, you may not see every chapters in the left tab. That's probably because you modify
    the ``index.rst`` file. To fix this, you need to save every ``.rst`` file.

How to make the `documentation downloadable as PDF <https://docs.readthedocs.io/en/stable/downloadable-documentation.html#>`__?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first thing you need is a configuration file.
`Here <https://docs.readthedocs.io/en/stable/config-file/v2.html#>`__ are the instructions to create it.
Once it's good, go to `this page <https://virtel.readthedocs.io/en/latest/manuals/newsletters/TN201707/tn201707.html#getting-started-with-sphinx-and-readthedocs>`__
and again follow the directions.
You don't need to download Pandoc but you will need to download `MiKTeX <https://miktex.org/download>`__ for your Ubuntu version.
Just follow the steps and it should be fine.
Then, continue to follow the steps from `this page <https://virtel.readthedocs.io/en/latest/manuals/newsletters/TN201707/tn201707.html#getting-started-with-sphinx-and-readthedocs>`__
up to *Build the PDF with TexWorks*.

.. attention::
    The mentionned TEX file is not located in the _build/latex directory but in the build/latex directory.

When pressing the green button with LuaLatex selected, we had this error:

.. code-block:: python

    ! LaTeX Error: File `cmap.sty' not found.

We solved it by writting this command in a terminal.

.. code-block:: c

    sudo apt install texlive-latex-extra

After pressing the green button and when the builder process is finished, a PDF will appear in another window.