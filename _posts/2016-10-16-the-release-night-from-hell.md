---
layout: post
title: The Release Night from Hell
tags: [dotnet, tech]
---
Friday October 14, 2016. That Friday was supposed to be like any other. I would be heading off to work, I would do what I was planning to do then head home and embrace the weekend. It would be a good Friday concluding a long week. There was only one thing that stood out that day; it was release day. I work in a two-man development team and my colleague was on holiday abroad. I was supposed to release what we have been developing for the past four weeks (although we're capable of doing continuous delivery to production, the major bulk of our release follows a four week cycle due to legacy dependencies), this wasn't really a big deal since I'd been soley releasing our application for quite a while. The only thing that stood out this time is that I would be alone. I thought it no big deal, I've done it in the past and so got the experience to handle issues. Also, during each release my colleague and I had finessed the process and continuously updated all documentation. I was ready for this (boy, was I wrong).

The day started with me arriving at work a bit later than usual, reason being that I had been working a bit late the day before to finalize a demo for the business. I was very excited that day. I was going to show off something new of version Y, and then I was going to start scheduling the release of version X of our application. I ran the demo at 10:00 AM, finishing off at 10:30 AM. The business were happy and I was happy. I grabbed some lunch, then headed back to my computer to get mentally ready for the release. I like to be efficient, so I thought, maybe I can code some of the changes that the business pointed out during the demo to version Y, before I get started of scheduling the version X release. I did the changes and finished off at around 02:00 PM. Alright, I thought, time to get started on the release. I set my status on Skype to Busy. Our application consists of five components that need to be deployed. We schedule the deployment of each component so that this happens automatically. This Friday it was a bit different though. One of the components had to be manually deployed due to a server change, we agreed that the manual trigger would be made by the infrastructure team since the server change was made by them. They would click the button on that one. The rest of the components would be automatically deployed at different time intervals. Of course, before I got to this bit, I had to start merging the code. I was in for a treat.

We use git and follow the git flow principle. During this release, we had done changes to the two major components of our application. Therefore I had two code bases which I had to merge. I started with the easy one. This component is the front-end part of our application. Usually, it's quite straightforward to merge this code, I seldom get any issues. I checked out master, pulled with a rebase then merged the release branch with master. Conflicts everywhere. I thought it was strange, I hadn't witnessed that many conflicts in this component before. Our front-end changes are usually straightforward. I started fixing the conflicts. Most of the changes here were made by my colleague, since he wasn't around, I sticked with the changes made in the release branch. I finished merging and pushed the code to master at around 03:00 PM. My plan was to finish everything and wrap up by 05:00 PM, so far so good.

I moved on to the next major component. This is the back-end component, which has many dependencies on a legacy system (which will be released this same Friday by the infrastructure team). The dependency artifacts that our component needs, are fetched through NuGet packages. There are two feeds the host the artifacts, one for the develop and one for the release/master branches. I checked out master, pulled with a rebase and merged release with master. Conflicts everywhere and project load failures, this was expected. Windows trailing whitespaces are considered conflicts by git in my editor (I learned later that I could configure Visual Studio to ignore this).

There were two sets of conflicts; all the packages.config and the project csproj files. For the packages.config files I could either choose to keep source or target. It didn't really matter what I chose here, since we have a script that I could run afterwards that would switch the NuGet feeds from develop to release then update all the packages.config files in the solution with the correct package versions. Running a restore of the solution would then do a restore and fetch me the latest packages from the correct feed. So I did that. The csproj files were a different story though. Instead of clicking on either keep source or target, I should have checked with my eyes what kind of conflicts there were and handled them accordingly. For some reason, I chose keep source. I believe that was the fatal mistake - which I did not realize at the time - that would change the outcome for the rest of the day.

I cleaned the solution, rebuilt and completed the package restore. Compilation errors. It must be the NuGet packages, there must've been a delay and the package versions weren't updated properly in the config files. I checked the versions that were in the package server, indeed, there were four module dependencies that had outdated package versions. They needed to get the version X. This is a known problem that we sometimes experience, for some reason, our script that updates the packages in the feed doesn't see a change and therefore doesn't trigger. The script checks for code changes that have happened during the last hour. I upped this value. After I did this, the versions were updated. Great, it's 03:45 PM and I'm on track. I did another restore of the packages and compiled the solution. Visual Studio tells me that it could not find some dlls. No compilation error, just that dlls are missing. Interesting, never seen this during our release before. 

## The Hell Begins

Dlls are missing. No compilation error. The words repeat themselves in my head. How is this possible? There must be some reference issues somewhere, but how? We never get this problem. No, it just can't be. I refuse to accept this error. It must be local, I'm pushing this. The build server will be green. The dice is cast. Let it go down in history, that at this point in time, I could've done many other things. Best case I could've reverted the commits, worst case I could've just deleted my local branch and started the merge all over again. But keep in mind, at this point it's 04:15 PM this Friday and I'm not even halfway through finishing the release. I really just wanted to finish this component and finalize the release and leave the office by 05:00 PM. Was it reckless of me to push the code? Yes. The build server fails and it fails magnificently. Compilation errors all over the place, nothing builds. Now, I knew that I wasn't going home by 05:00 PM.

## The Pressure Amounts

I was supposed to prepare this component for release and inform the infrastructure team, so that they could manually start the deployment after the server change had been made. I went over to the team and told them that I would be here for a while. We chuckled a little bit.

Alright, so at this point I messed up the remote master branch. How bad could it be? Should I attempt to rollforward and try and fix the reference issues, or should I rollback - revert the commits? I decided to revert the commits. There were four commits made in total to master, one of them a merge commit. The reason to this, is that I merged the code, "resolved the conflicts" (obviously didn't) then ran our NuGet script to update the packages.config files. This required several additional commits. Alright, I needed to revert four commits that were pushed to master.

``git revert --no-commit HEAD~3^..HEAD``

That should do the trick. Not really...

One of the commits was a merge commit, and this doesn't work for merge commits. I needed to run this for the last three commits, then run something else for the merge commit. At this point, it occurred to me that I should probably create a backup branch of master and play around with that one. I definitely shouldn't pour fuel into the fire. So I created a master/backup and started experimenting.

``git log``

I grabbed the hash of the merge commit. Then

```
git revert --no-commit HEAD~2^..HEAD
git commit -m "Reverting the last three commits."
git revert -m 1 <commit-hash>
git commit -m "Reverting the merge commit."
```

That should do the trick in theory. Except it didn't for me. I ended in a strange state. I look at the clock and it's 07:00 PM. We have business people who would be doing verification of version X at around 09:30, I feel a bit of pressure rising and start thinking of the worst case scenarios. What happens if version X is not released? If I'm unable to release this, then the main system would have to postpone their release otherwise there is a high chance that our existing version in production will not work after the main system's version X release. This is a case where rolling forward is the only vaiable option.

## Time flies

It's 08:00 PM. I explain the situation to the infrastructure team. They have their hands full releasing the main system. I go for a little walk in the office, go downstairs, grab a glass of water and eat a couple of apples. I feel alone, it's not the best feeling. How could this happen? Why am I alone? How did we allow this situation? A release is a responsibility, we should have better contingency plans and optimally we shouldn't let people be alone in such situations. We are an autonomous team within the company boundries, I'm the team leader so I take full responsibility. I make a promise to myself that I will not let this happen again.

An idea pops to my head.

## The turning point

The remote master branch is messed up, but the release branch is working. Why not just use the release branch? What is the probability that the current master and the release branches are different?

## It's not over yet
