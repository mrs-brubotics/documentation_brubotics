3. Introduction to MRS Software Extensions
==========================================

In this chapter we will present you how to install BruBotics MRS Software.

In the previous part we learned how the default CTU MRS software is constructed and how it can be used. In this part, we will learn how we can use the mrs uav
system as a starting point to quickly test our own UAV algorithms. We will show in a tutorial style how to do this step by step. The reader is strongly advised
to follow the explanations in this report by simultaneously trying out the examples that can be found on our github. You learn and remember more by trying
yourself.

.. important::
	Please, do not push your commits to our github when you try to reproduce these results! In the case you obtain a different result, always contact Bryan and
	explain how to reproduce it. It might be a bug to be solved.

The goal is that you contribute your tutorial content to this report and your software to `our github <https://github.com/mrs-brubotics>`__. Keep the following
in mind:

* Remember that many researchers (Brubotics and CTU) will update the software and this document. In case of ideas to start building new software, :red:`first discuss
  your ideas and problems with those researchers`. The same holds when you encounter problems to be solved. Also know that you can ask questions by
  opening a new issue on the MRS github (visible by everyone) or our github (private, not visible by everyone) or send en email to the contributors of the projects.
  :red:`So don’t reinvent the wheel, instead ask advice to the creators to lose less time and learn more.`

* Our software is based and heavily dependent on the software of the CTU MRS group. :red:`Therefore it is very important to have pulled the latest CTU MRS software
  (see` :ref:`Chapter 4 <4. Workspace Git configuration>` :red:`) before pushing your new code to our github. Same can be said for code on our github`. Make a habit to
  update CTU MRS software and our MRS brubotics software :red:`each day` you work on the project and right before you do final tests of your software stack and the git 
  push. Also try to at least :red:`twice per month` delete all code from your PC and do a fresh installation of all the software. Since you pushed it on git, it means it
  should be working again, hence it’s a good test.

* Before you push commits to our github always make sure the software is tested and confirmed "working properly" by all your team members. For each running
  project it is advised to work on separate git branches per project.
  :red:`[todo by Bryan: I think we could also setup a pull request to figure out how that works improve first. Qo i can check work of students and accept if is ok, reject and say what they have to change.]BC`


* When starting developing your own software you should directly start writing in this part a tutorialstyle documentation about it. First discuss with the
  responsible researchers (e.g. Bryan) where to best place your content and create a logic structure it in this report. You should write as if youwrite a
  tutorial for people with very basic ROS knowledge. Explain for each task how you createdyour software step by step. Do it similar as the ROS book you read, by
  guiding the reader step bystep. Make sure it is clear how to reproduce the experiment, what to launch, which parameters toset, etc. Better to give more details
  than to explain things too high-level. This does not mean you have to copy paste all code in here.
  :red:`[we should specify how they should refer to code: via links, to commits, a test package of our own?]BC`

* Since this software and documentation is continuously updated, it is important to regularly retestif your software still works as expected and if your tutorial
  content is up to date with the software. Make a habit to regularly check for new issues and code changes in the CTU MRS software, since they will affect your
  code too.

* If you think the tutorial or software that you did not develop yourself could be improved in some way, put your comment here and discuss first with Bryan.
  For software changes, please open a new issue first in which you explain the bug and your idea. For smaller changes in the tutorial content,do not delete the
  original content, but reformulate the part using your initials (e.g. :red:`["I reformulate the part like this. I like it more because of
  these reasons."]BC`). Bryan will regularly go through thereport to accept or decline these changes.