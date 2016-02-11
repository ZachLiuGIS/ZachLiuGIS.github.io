---
layout: post
title: "How I made my first contribution to Django "
description: "I have been working with Django for a few years and decided it was time to do something for the project.
Here is how I made my first contribution by fixing a ticket from beginning to the end."
category: django
tags: [python, django, github, open source]
---
{% include JB/setup %}

I have been working with Django for a few years and decided it was time to do something for the project in return.
I have never made contributions to a project this level so I was not very certain at the beginning. Fortunately, 
there are clear documents and great people ready to help in the community. So it went out pretty smoothly. So here
is a work through of my whole process, from learning how to do it, to my first pull request was closed.


## Read how-tos

First I went to [Contributing to Django](https://docs.djangoproject.com/en/1.8/internals/contributing/) page, which is 
a summary of how to make contributions.

Then I read through [Advice for new contributors](https://docs.djangoproject.com/en/1.8/internals/contributing/new-contributors/)
on the Django documentation to get some guidelines. 

I signed the Contributor License Agreement from the page and sent by email. 
I got it responded pretty quick, so I was ready to go.

Reading to many documents can be boring, so I found two tutorials ([Writing your first patch to django](https://docs.djangoproject.com/en/1.8/intro/contributing/), 
[Working with Git and GitHub](https://docs.djangoproject.com/en/1.8/internals/contributing/writing-code/working-with-git/)) to get a better idea 
what the whole process looks like, and then just started to get my hands dirty by learning by doing.


## Get a Copy (fork) of Django Source Code
I then went to (https://github.com/django/django)[https://github.com/django/django] to fork
the repo into my own account. So I can have my own copy of the source code.

After the fork, clone it into my development machine using 

`git clone git@github.com:github_name/django.git`.

Then I tell my local repo where is the original "upstream" remote by running

`git remote add upstream git@github.com:django/django.git`

It is always a good habit for setting up a virtualenv for every project you work on, so I created one called django
using virtualenvwapper

`mkvirtualenv --python=python3 django`

django should be installed in the virtualenv for development also.

`pip install -e /path/to/your/local/clone/django/`

The initial preparation was all set at this point, now I needed to find out what I could work on.


## Find a ticket

Find a ticket you are comfortable doing will be the first step. Since this is my first time, so I looked into those
marked easy-picking on page [Easy-Pickings](https://code.djangoproject.com/query?status=assigned&status=new&easy=1&col=id&col=summary&col=status&col=owner&col=type&col=component&order=priority).

The ticket I found was [ticket #26179](https://code.djangoproject.com/ticket/26179).
It asked to drop a validation check during model field value assignment because the task should be left for database.

It looked doable so I logged in using my github account and assign the ticket to myself. After assignment I became the
owner of the ticket.

Then I create a branch ticket_26179 and base the work on upstream/master as specified in django document.

`git checkout -b ticket_xxxxx upstream/master`


## Run Test Before any Coding

Test should be run before any coding to make sure a clean start (on ticket_26179 branch).

cd into `django_directory/tests`

then run `PYTHONPATH=.. python runtests.py --settings=test_sqlite`

It took a few minutes and the test said ok. So it was time for some coding.


## Make the Changes Asked by the Ticket and Submit Changes

Before I made fixes for the ticket, I created a small django project to play with the current behavior that needs to 
be changed, just to get familiar with the code. A demo project will help trouble shooting and test the changes along
the way too.

After playing with it for a while, I made the changes asked in the ticket, and then after the new changes are confirmed 
by the demo project I created, I committed the changes to the branch by:

`git commit`

In the interactive window for commit message I used the code style in the doc with a title and a description.

```
Fixed #26179 -- Remove null assignment check for non-nullable foreign key

A longer description of the commit.
```

Close the window and the commit will be saved in git log.

All the changes I made are in this [Pull Request](https://github.com/django/django/pull/6115)

To create the pull request after code changes. I pushed the commit first using

`git push origin ticket_26179`

Then I go to my forked repo, find the branch and click *Compare and Create Pull Request* button.


## Don't forget to run test after code changes

I actually forgot to run test and realized it after I sent the pull request. *The ticket saying no test needed does not 
mean you don't need to test after changes* I rerun the test, and I got three failures. Ouch.

I checked the error messages and located the failed test functions and lines. It looks like my changes caused expected 
ValueError to not be raised because I dropped the rasing error behavior. So I removed the tests thinking they are not 
needed any more.

Bottom line is *run test before committing and submitting any code*.


## Read Response in the Pull Request

After pull request, I got response pretty quick with comments. Here are my rookie mistakes:

- Instead remove those failed tests, I should change tests for changed behavior. So the tests should validate None
assignment for foreign key field should not raise ValueError. 

- I should update release document since this changes will not be compatible with previous versions.

- I need to updated the ticket page to mark *has patch* to yes since it includes a patch.

- I need to include a link to Pull Request in the ticket page.


## Take Further Actions

After seeing the comments, I saw where I need to improve and made changes accordingly. Run tests again and made sure 
everything is ok. Then I updated the ticket page and also pull request to notify you have made further changes.

To update your pull request, simply commit and push to the same branch. Your pull request will be updated automatically.
Nice!


## The Pull Request was Closed at the End

After the changes I did the second time, I got response that my fixes to the ticket are all right and the pull request
was merged into the code base. Overall It took me two days (about two hours total work) and I was pretty glad I made my
first contribution to Django.


## Summary

So after the steps above, I got my first contribution to Django project finished and the sense of community made me feel
good. Hope more work will be coming down the road.