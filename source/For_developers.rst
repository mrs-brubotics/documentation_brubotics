For developers
===============

Definition of authors' comments
-------------------------------

The following syntax allow you to write a comment in color with your initials:

.. code-block:: html

    :red:`[This is a comment.]INITIALS`

And it will appear like this:

:navy:`[This is a comment.]INITIALS`

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

If you are reading this, it means that you already know how to visualize this documentation.
However, you can still read `these instructions <https://github.com/mrs-brubotics/documentation_brubotics>`__ in the README file.

How to edit the ReadTheDocs website ?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It can be easier for you to code your website by using Visual Studio Code with an extension called reStructuredText wich is useful to previsualize your
website. It has a syntax highlighting tool.

The only files you need to modify are ``conf.py`` and all the ``.rst`` files in the ``source`` folder. Once you want to update your documentation, use the following
commands from your directory:

* Before all, we recommend you to run this command to update your local repository and get the newest code:
    
.. code-block:: shell    

    git pull

* To check what files wich have been updated since the last commit you can use that command:

.. code-block:: shell

    git status

* To update your repository, adapt and run these commands

.. code-block:: shell

    git add (use tab key and type the first letter of the files to commit or use git add -A to directly stage all files)
    git commit -m "Provide a clear explanation of your commit. People who did not make the change should understand the issue you solved."
    git push

Please refer to section :ref:`2.4 <2.4 Working with Git>` to understand why we use these commands.

.. note::
    When visualizing the documentation after running ``make html``, you may not see every chapters in the left tab. That's probably because you modify the ``index.rst``
    file. To fix this, you need to save every ``.rst`` file.