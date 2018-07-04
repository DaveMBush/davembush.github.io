---
title: How to Estimate Software Projects Like a Pro
tags:
  - estimating
  - project management
url: 4024.html
id: 4024
categories:
  - Scrum
date: 2016-08-30 06:30:00
---

We’ve all been there.  Either at the micro level or at the macro level.  Business wants to know, “How much is this going to cost me?”  And as software developers, we all know the answer is, “more than you were expecting.”  We also know that whatever number we give will probably be wrong for a number of reasons.  Chief among them is that no one really knows what they want until they see it. And yet, there has to be some way of providing business what they need and still allowing for unknowns. So what follows are a few tips on estimating that help you estimate software projects like a pro. <figure>![](/uploads/2016/08/image-2.png "How to Estimate Software Projects Like a Pro") Photo via [Visualhunt](//visualhunt.com/photos/business/)</figure>

<!-- more --> 

You Don’t Know What You Don’t Know
----------------------------------

This tends to be the most famous argument for not giving an estimate.  Or for giving an estimate that is all but meaningless. “My gut says this will take a month, so I’m going to say four months because I really have no idea.” Well, that might be a safe estimate, and you might be right.  But if the business is looking for a number to use for a budget, they are going to learn that you pad your estimates.  So, a better answer is, “Are you looking for a rough ballpark or are you basing your budget on this?” And if they say they need a somewhat accurate number, your answer should be, “I really don’t know enough about the project.  Could we break it down into its component parts?

Virtually Walk Through the Project
----------------------------------

This will work regardless of the project size and regardless of if it is just you or a team. Here’s what you need to do, once you’ve broken the project down into the component parts, ask yourself, “What is the next thing I would need to do to get this project moving?”  You write that down, come up with an estimate for how long that will take, and then ask yourself the question, “Assuming I’ve completed what I’ve listed so far, what is the next thing I need to do?”  And you just keep doing this until the project is (virtually) done. You want to be careful as you are doing this to:

1.  For estimating actual work at the programming level, make sure none of your tasks take more than 8 hours to complete.  I personally aim for 4 hours.  Even if you use story points instead of actual hours, I’m sure internally you have some idea of how many hours a story point represents for you.  Size the work accordingly.
2.  For estimating at the more macro level, I recommend dividing the project into 2-man week chunks.  Basically a sprint.
3.  Don’t forget the obvious

1.  Setup time
2.  Creating tests.
3.  Bug fix time.
4.  Broken dependencies

Everything takes twice as long as you think it will.
----------------------------------------------------

This rule of thumb has served me well over the years.  But it isn’t a hard and fast rule.  It often depends on the client.  I switch clients frequently because I am a contract programmer.  If I’m new, I’ll provide estimates with the 2x multiple factored in.  But as I learn more about the people providing the requirements, I’ll tweak that factor appropriately. I had one product owner I worked with who I learned to provide a multiple of four to because I only heard half of what he was trying to communicate.  I’m not sure where the communication breakdown was, all I know is if I multiplied by four, I was much better at being able to manage his expectations.

Track Everything
----------------

Even if you implemented everything I’ve recommended, you are still likely to fail in the long term because you’ll never learn from your mistakes. So what might you track?  Well, at the personal level, you want to track how close your estimate was to reality.  Over time you’ll learn that you tend to be off by so much.  And as you learn to estimate and track, I would expect your estimates to account for more items so that your multiple gets closer to a factor of 1x than the 2x I recommended you to start with. At the more macro level, you want to track the team.  Please, don’t track individuals.  Let the individuals do that.  Provide training so they can get better.  But if you do the tracking for them, you’ll destroy moral. But you want to know, when the team says they can do X in a week, that probably means it is going to take a week and a half… or whatever it ends up taking. I’m working on a project now where, had I been the project manager, I would have doubled all the estimates I was given.  Oh well, they’ll find out soon enough.  Although I would have expected this particular PM to know better. You also want to track items you may have forgotten to put in your plan.  You’ll add this to your “estimation checklist” which I’ll discuss next.

Estimation Checklist
--------------------

You want to create a checklist that you can use to make sure you’ve covered everything that needs to be estimated.  Along with the items I included above, here are some others you might want to include:

1.  Database Refresh side effects – yes I know, each programmer SHOULD have their own copy of the database, but I’ve yet to work for an organization that does this.  Because we don’t have this on my current project, we’ve lost valuable morning time twice this week waiting for the database we are using to get updated.  At least we aren’t all working against the production database.
2.  Version Control Management – this includes branching, merging, pull request, and code reviews.
3.  Holidays and Vacations
4.  Any learning curves that must be mastered.

Management Doesn’t Like the Estimate
------------------------------------

There have been a few times when the manager I’m working for doesn’t like the estimate I provide.  In my experience there are several reasons for this. First, and almost always, if I’m right, the project is going to take a lot longer than they expected, or longer than they’ve been told to do it in. Second, they are used to estimates that are half as long.  The problem isn’t that my estimates are wrong, but that their expectations have been set lower. What to do? Well, I take them to my list and show them where all the time is.  Because I can document where the time is going to be spent, I rarely have to do more than explain how I came to the estimate I did. If they still bulk, I simply have this conversation, me: “So, you think my estimates are high?” them: (yes). me: “Do your other programmers write tests for their code?” them: (no) me: “When you compare their time to the time I’ve said I’m going to take, do you account for bug fixes?” them: (no) me: “If you did account for the bug fixes, would their estimates be more or less accurate?” them: (less) me: “Do you believe the way I’ve proposed to complete this project will result in less bugs than you normally see?” Well, you get the point.  I’ve yet to have to have a conversation that went into that much detail.  And the more you track, the easier this conversation becomes because you can just say, “I’ve been tracking my estimates against reality now for N years and I’ve found that I normally am within X% of my estimate.

Tools
-----

There are a couple of tools I really like for estimating.  The first one is the Mind Map.  If you aren’t familiar with Mind Mapping, it is a way of just getting your ideas down on paper without having to worry about the structure or the order.  All you concentrate on is relationships.  This is a great way of breaking a project down into the smallest possible units of work. Once you have a mind map of your tasks, you can put the individual tasks into a Kanban Board.  You want a board that can provide the ability to attach estimates to the tasks and will show you a burn down/up chart based on how much work you’ve completed. For Mind Mapping, I currently use [Mind Mup](//www.mindmup.com). For Kanban, I use [Trello](//trello.com/) with [Plus For Trello](//chrome.google.com/webstore/detail/plus-for-trello-time-trac/gjjpophepkbhejnglcmkdnncmaanojkf?hl=en). I wish there was a tool that married the two ideas.  That way I could use Mind Mapping for Epics, Stories, and defining tasks and I could use a Kanban board for tracking work.
