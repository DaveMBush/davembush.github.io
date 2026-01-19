---
title: How AI Makes Me a Better Programmer
date: 2026-01-19 13:55:43
tags:
  - AI
  - programming
  - personal development
---

## How AI Makes Me a Better Programmer

### Not what you think

At least I hope it isn't otherwise why bother writing this?

Most people probably think that when I say AI makes me a better programmer that it makes me more efficient. That I can get more done with less effort. And while that is also true, I find that AI has forced me to become a better programmer in ways that will continue to benefit me even if AI is just a fad and we all start writing code by hand tomorrow.

<!-- more -->

### The main problem with AI generally

Despite all the hype around AI. The one thing that has become very clear to me is that AI will tell you what you think you want to hear. It is the perfect "Yea Man". To get the full story on any topic you have to be willing to challenge the AI, to question its answers, and to dig deeper than the first response it gives you. And it knows, because it just wrote that last sentence for me 🙂.

This is a critical problem when we are discussing social issues. Or any topic that has a multitude of perspectives.

Just as an example, and trying to stay away from anything particularly sensitive, if I ask AI how effective statins are at preventing heart attacks, it will almost certainly give me the answer that your doctor has been trained to give you. "Roughly 20-25% reduction in risk."

Then you ask what are the chances of a heart attach with and without statins and you find out that even for a severely high-risk patient, the absolute risk reduction is relatively small. That is, if you don't take the drug, your risk is 20-30%, and if you do take the drug, your risk is 13-20%. This means you reduce your risk by only 7-10 percentage points.

I have to admit, asking now, compared to when ChatGPT first came out, I received a much more complete answer the first time I asked. But it still left out important details. For example, it acknowledged there are risk but it didn't go into much detail about those risks.

### Programming is different

Thanks to many years trying to make sure humans don't make any more mistakes than can be helped by using best practices, we already know how to make sure AI generated code is reliable. And this is both why we are able to use AI for programming and why it has made me a better programmer.

### Planning

Before I started using AI for programming, I was inclined to figure out what I needed to do as I went along. I wouldn't plan at all. I'd jump in and see where things would lead.

But now, for larger projects, I'm forced to create a plan before I start coding. I can use AI now to help me create the plan, but it is still MY plan and I have to review it.

I find using frameworks like the BMad method critical in helping me develop a plan.

### Testing

Like many of you, testing has always been an after thought. I've gotten better along the way, but every organization I've worked at, until recently, has had an excuse for not testing thoroughly.

But I've learned that if I don't have as much testing as possible, AI is likely to change code that was working because it has already forgotten what it is for.

By making sure I test in as many ways as possible, I reduce the risk of AI introducing bugs or changing the behavior of my code in unintended ways.

Once again, we can use AI for this too.

And I'm not just talking about Unit Tests. We also need a good set of e2e tests that cover the full functionality of our application. Also, it is important to make sure our GUI hasn't changed so we need something in place to make sure that even though we have e2e tests that work we still have a screen that our customers can use.

One thing I've started doing is making sure I either create or have AI create unit tests for the code I'm about to write before I write the code. That it, I force AI to do Test Driven Development. This not only produces the tests I want but it helps the AI produce the code you expect assuming you review the tests prior to having AI write your code.

## CI/CD Pipeline

I've played around with CI/CD pipelines in the past but once again, AI has forced me to take them more seriously.

Even as a single developer, I use the CI/CD pipeline to ensure that my code is always in a deployable state and that nothing changed on me unexpectedly.

Now that I'm doing this, I'm getting better at programming the CI/CD pipeline.

## Code Reviews

I don't know anyone who likes reviewing code, or having their code reviewed. But AI doesn't care. I can critique AI's code all I want and it will always reply with "You're right, I'm sorry, I should have thought of that."

But if I'm going to be sure that the program I'm writing is code that does what I have in mind, I have to read the code myself. Yes, there are AI tools for code review. And, I use them. But at the end of the day, I am the one responsible for the quality of the code. But that it does what I want and that I can maintain it later.

## Conclusion

With all these things we need to do, you might think that it would be faster to just write the code without AI than to use it if you are going to go through all these steps.

But, you are forgetting we SHOULD have been going through all these steps all along. And while you could just slap something together without following these steps and without reviewing the code, in the end it will either take time to code it "right" now or it will take time to fix it later.

AI has simply made these best practices more apparent and easier to follow, which in turn has made me a better programmer.
