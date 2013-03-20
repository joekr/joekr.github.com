---
layout: post
title: "Git: a powerful front-end to Subversion"
date: 2012-01-28 22:13
comments: true
categories: [Git, SVN, Software Development]
---
[Git](http://git-scm.com/) is a great version control system. I have used it for some time on many different projects and I really enjoy working with it. My life is bit simpler because of Git.

When I had to move back to [SVN](http://subversion.apache.org/) I didn't really like the idea of losing local features branches, stashing , and easy merging. Luckily, Git has the a [svn command](http://schacon.github.com/git/git-svn.html) which allows me to use Git as my front end to SVN.

What do I mean by front end? Basically, I use Git on my development machine, and use the git svn command to push/pull code from the SVN Server. Git is how I interact with SVN.

Why, if SVN is already being used, would you use Git? SVN is used in many code bases I have been working on lately, but that doesn't mean I can't still use the advantages of Git.

**Branching and merging:** I'm sure, like me, you have tasks that change quickly. On a daily basis I need to switch from working on a new feature, fixing bugs, etc. Git allows me to switch from a feature branch I'm currently working on, to a new branch for a bug/fix then merge the new branch and push upstream. Then finally get back into my larger feature branch.

Typically with SVN or other CVSs this is a bit more challenging. Many times I would have to say "Hold on while I get to a good stopping point". With Git I temporarily save my changes (Stash - will talk about a bit later) and create a new branch and start working.

**Working Locally:** I don't have to comment feature code out just to make sure it is checked in. Since Git is a [DVCS](http://en.wikipedia.org/wiki/Distributed_revision_control) I can store all my changes locally, to later be pushed to SVN when ready. This allows me to [check in early and often](http://www.codinghorror.com/blog/2008/08/check-in-early-check-in-often.html) locally without messing up a co-worker by having crazy comment code strewn about the code base. I can make sure my feature is fully ready, and tested, before pushing upstream for all to use.

**Stashing:** As mentioned before Git allows me to temporarly check in current code changes. Many times someone will point out a trivial issue in the code which needs to be in production "now". With [git stash](http://book.git-scm.com/4_stashing.html) I can save whatever it is I'm working on with out having to check in non-working code, again saving me time to not have to be in "a good stopping point".

 

There are many other features Git has, these are the ones that make it a great front end for SVN. Using git svn not only saves me time, but it allows me to keep my code out of the master SVN repository until it is good and ready. I can count the number of times I have been working in newly pulled code only to find something is broken because line 23 in some file should have been commented out. Since I don't like when others do that I choose to use Git to help make everyones lives a bit easier. What is the old saying: Do unto others as you would have them do to you.

 

Two tutorials that helped me:

+ [http://www.viget.com/extend/effectively-using-git-with-subversion/](http://www.viget.com/extend/effectively-using-git-with-subversion/)
+ [http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)
