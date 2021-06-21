1. How to use ReadTheDocs
=========================

In this chapter, we will explain the basics of ReadTheDocs.

1.1 How to produce the ReadTheDocs website
------------------------------------------

To create this tutorial, we used a documentation generator called Sphinx and reStructuredText. We refered to the `ReadTheDocs Documentation <https://docs.readthedocs.io/en/stable/index.html#>`__
and `ReStructuredText primer <https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html>`__.

1.2 How to open the ReadTheDocs website
---------------------------------------

As said in the chapter `Getting started with Sphinx <https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html#>`__, you have to open ``index.html`` in your web browser
to see your documentation. This file should be located here: ``build/html``.

1.3 How to edit the ReadTheDocs website
---------------------------------------

The only files you need to modify are ``conf.py`` and all the ``.rst`` files in the ``source`` folder. Once you want to update your documentation, use the following commands from your
directory:

.. code-block:: shell

    git commit "Name_of_your_commit_"
    git add "name_of_modified_files"
    git push

Please refer to section :ref:`2.5 <2.5>` to understand why we use these commands.