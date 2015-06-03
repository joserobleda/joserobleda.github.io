---
layout: post
title: "What's Your Development Workflow?"
modified:
categories: development
excerpt: One of the many things I've learned over the last few years is that a bad development process is a big time waster. This post is to share the latest version of the workflow we're using in dokify and aims other people to share their thoughts about it. Also will be nice to see other posts explaining how other projects work.
comments: true
tags: ['development', 'git', 'workflow', 'code', 'project']
image:
  feature: feature-coding.jpeg
  credit: Luis Llerena
  creditlink: https://unsplash.com/negativespace
date: 2015-06-02T21:30:37+02:00
---

One of the many things I've learned over the last few years is that a bad development process is a big time waster. This post is to share the latest version of the workflow we're using in [dokify](https://dokify.net) and aims other people to share their thoughts about it. Also it will be nice to see other posts explaining how other projects works too.


### 0. The "why"

Whether you think the feature is bullshit or not, you **always** must evaluate if it really need to be done. Why anyone wants you to code this? Whats the deepest motivation of make that feature?

Sometimes you have to comply with a change without being able to fully understand the why, but in the 99%, if you really want to know it, you can find the source and talk with the guy who requested it.

Only when you're sure your completly understand the "why" you should continue to the next step or, maybe, move to a next feature ;)

> The best code is no code at all

### 1. Just draw some lines

Do not think technically here. I think the most important thing in the first step of every change, enhancement, feature (or whatever you want to call it) is to put your creativity first. As developers we love to code, and too many times we limit ourselves by thinking in the code we need to write too early, and then discard some ideas just because our first thoughts. What a big mistake.

**Try to get as many ideas as possible** at this point. Have meetings with any stakeholder you need and do brainstorming with them.

### 2. Read and measure

Without discarding any of the generated ideas make an initial check against your possibilities.

**Read the headlines of your codebase**. Usually the way you write a feature will be based upon existing code, and that should not prevent you doing things in any form, but lets you know if a refactor will be needed to make the change, and allows you to consider the time to do it. With that early information, you will be able to determine more precisely the time needed to finish the task if you go through that way.

**Review your technology**. Meaning, for example, that you have a very different amount of work to do if you have a redis server already in running in your infrastructure than if you don't.

### 3. Expose and decide

Now you have a few options to go and a reasonable perception of how hard to implement each one, now **is the moment to choose**.

A list of pros and cons will make easier to decide. Also, you can, and maybe you should, have a meeting with any others involved. They can give you others points of view to make the smarter choice possible.

Of course there will be a lot of times when not everybody will agree on which is the best option to go, but you have the responsibility to make consensus on what to do, even if there are people who thinks there are better options. I think this is hard, but it's worths it. This does not mean that everyone agrees with your vision, but to make sure they all know the "why". Even if you don't think the same way there are red lines from you and form others you'll never cross if consensuous is necessary. **It is not a guarantee of the best choice, but makes a safer choice**.

Now you have **the way to go**.


### 4. Pave the way

Don't code yet. Yes, we want to code because we love to code. But hold on, there are a few things to do before if you want to be efficient.

I think it is impossible to predict all the changes needed in a medium-large feature, there a lot of chances you can code until a point in which you can't continue and need to re-do some work. We need to prevent this as much as possible.

Split. Split. Split. I'm sure there is a way to split your feature into a smaller pieces. Divide a task into a small steps is a winner strategy. You have more control over the components, its easy to prevent any future problem, easy to talk with teammates and of course easy to implement.

> Don't try to code anything you can't hold in your brain's RAM - me, now

If you use a software like Github, create a milestone and write the pieces as issues inside. A bonus gift will be that you can track the milestone progress easily.

### 5. Go through the details

Now that you have pieces clearly described, then make sure you can complete the flow from the beginning to the end of the process without magic. Magic appears when you have a variable that comes from nowhere, when looks like you are able to store data in your database without a web controller, when you get a user input data without a proper view for the user enter that data, etc...

I only know one way to avoid this: write a detailed to-do list before you code. Be as precise and rigorous as possible on doing this.

Optionally, depending on your confident with the project and codebase, you can send it to review. If another member of the team can understand and validate your to-do list, you've done the most important part of the job.

### 6. Write Specs and/or tests first

This is a controversial point, and that's a fact. Tests have a supporters and detractors, and there are tons of ways to do it.

I am not ashamed to say that, in general, I don't like to write tests. But anyone with experience in software development knows it will be necessary sooner or later.

Maybe you feel like your using too much time to write tests, but as time goes on, you'll see clearly how important it is.
And that said, I even think this is a wrong assumption in the first place, since if you apply TDD the right way you can write, check and repeat even faster.

Depending on your codebase you'll be able to write more or less tests, but make sure you cover at least for the most crítical pieces.

### 7. Now you're coding!

Finally! Time to code!

![coding](/images/coding.gif)

Now you're almost copying from your to-do list. Perhaps a few things can come up, and of course you can deal with unpredictable problems, but you just minimized the damages this way.

### 8. Review

When you are done with code, a good practice is to send your changes to review. If you haven’t done this before, it can be hard at first, but the diffs at this point should be small, and your teammate will read easily your changes. You can define, with your team, a coding guideline and adopt some code standard, this makes it even easier.

With Github, you can send a pull request and assign someone to review. You can even assign yourself to look over it one last time. Of course, this point can be skipped if the change is too small...  
*because we have a QA!*

### 9. Submit to QA

Unlike in the previous point, I don't want a code review here in any form. I want to check if a user can use it without any problem. It is very important that a different person tests the final version of the software.

If you have a dedicated role in your team to do that it will be really useful if the person who are doing this have UX skills, which leaves its mark on the final result.

### Conclusions

This version of the workflow is the most detailed I'm able to write right now but, depending on your problem, you should skip or minimice the steps. In my opinion there are not silver bullets, but I think this aproach could fit in a lot of development teams with a minimum adaptations.

**Whether you like this or not**, please, leave me your thoughts in a comment and share to make everyone does the same, hope we can improve it!
