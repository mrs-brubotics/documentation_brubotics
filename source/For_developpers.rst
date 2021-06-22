For developpers
===============

.. role:: raw-html(raw)
    :format: html

2.8 How to use ReadTheDocs
--------------------------

In this chapter, we will explain the basics of ReadTheDocs.

2.8.1 How to produce the ReadTheDocs website
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To create this tutorial, we used a documentation generator called Sphinx and reStructuredText. We refered to the `ReadTheDocs Documentation <https://docs.readthedocs.io/en/stable/index.html#>`__
and `ReStructuredText primer <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`__.

2.8.2 How to open the ReadTheDocs website
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Before all, run this command in your directory: ::

   make thml

Then, as said in the chapter `Getting started with Sphinx <https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html#>`__, you have to open ``index.html``
in your web browser to see your documentation. This file should be located here: ``build/html``.

It can be easier for you to code your website by using Visual Studio Code with an extension called reStructuredText wich is useful to previsualize your website and it
has a syntax highlighting tool.

2.8.3 How to edit the ReadTheDocs website
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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