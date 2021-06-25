For developers
===============

.. role:: raw-html(raw)
    :format: html

Definition of authors' comments
-------------------------------

The following syntax allow you to write a comment in color with your initials: ::

    :raw-html:`<font color="Name_of_the_color">[This is a comment.]INITIALS</font>`

You can hind a list of available colors `here <https://htmlcolors.com/color-names>`__.

Don't forget to add these 2 lines at the top of your ``.rst`` file, just under the header: ::

    .. role:: raw-html(raw)
        :format: html

You can see here the definition and the rendering of the current author's comments:

:raw-html:`<font color="Tomato">[This is a comment from Bryan Convens.]BC</font>`
    
:raw-html:`<font color="Teal">[This is a comment from Kelly Merckaert.]KM</font>`
    
:raw-html:`<font color="Maroon">[This is a comment from Zakaria Lakeel.]ZL</font>`
    
:raw-html:`<font color="LimeGreen">[This is a comment from Titouan Tyack.]TT</font>`
    
:raw-html:`<font color="RoyalBlue">[This is a comment from Jonathan Vogt.]JV</font>`

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

* Before all, we recommend you to run this command to update your local repository and get the newest code : ::
    
    git pull

* To check what files wich have been updated since the last commit you can use that command : ::

    git status

* To update your repository, adapt and run these commands

 .. code-block:: shell

    git add (use tab key and type the first letter of the files to commit or use git add -A to directly stage all files)
    git commit -m "Provide a clear explanation of your commit. People who did not make the change should understand the issue you solved."
    git push

Please refer to section :ref:`2.4 <2.4 Working with Git>` to understand why we use these commands.