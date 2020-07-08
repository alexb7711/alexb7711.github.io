---
layout: post
title: Updating Suckless Utilities
categories: C/C++
---

I never considered the scenario of wanting to upgrade my build of st, dwm, or any other "forked" project that I may have until I wanted to upgrade st to the latest relase in order to try the new [scrolling functionality](https://www.youtube.com/watch?v=sdeX2S2uOeA) that they have added. My brain immediataely accepted the fact that I was going to download a new copy of st and repatch everything I currently have... but that isn't very sustainable for long term use especially if you want to update every so often. I thought about it for an hour or so and was struck with the simple solution of just using git's merge feature. The thing is on how to get it configured simply. Like they always say with programming, "think lazy".

# Creating Another Remote Repository
Much like how some people use this to keep multiple repositories up to date, multiple remote repositories are also very handy for updating (basically the same thing stated in different words). To take advantage of this feature, you can run: 

```
git remote add -m suckless https://git.suckless.org/st
```

to create anothe remote repository called suckless. The `-m` flag is set up to point at remote’s <master> branch.

Now if you run 

```
git remote update
```

You can make sure you are up to date on HEAD of suckless's master.

# Merging Your Source with Suckless's
To do this, I like to create a branch to make sure I don't destroy my master branch:

``` 
git checkout -b suckless
```

Then you simply have to run:

```
git merge suckless/master
```

and git will attempt to merge your code with that of suckless/master. You will most likely get conflicts, so be sure to checkout my post on how to set up neovim as a merge tool.
