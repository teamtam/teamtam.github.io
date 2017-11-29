---
title: Being Done Transcends Agile
description:  "Why being \"Done\" should not just be an agile thing"
author: Benjamin Tam
categories: Agile
tags: [Scrum, Done]
comments: true
---

While the words _agile_ or _scrum_ are now part of the vernacular of the software industry, the reality is many organisations are somewhere on the spectrum between it and a more traditional approach. Some purists may even view _agile_ and _waterfall_ as being binary and mutually exclusive. Regardless, if there is one idea that I think should transcend ideology is that of the _Definition of Done_ (DoD).

I remember my first experience as a team leader many years ago before agile became the de facto standard and I tried to steer the ship towards agility. My team had learnt gradually by osmosis what my expectations were when completing a feature. However, when a new team member joined the fray, he objected to my feedback that he wasn't actually _done_ for some work he had performed. In hindsight, a DoD would have obviously helped mitigate or even prevent an uncomfortable conversation.

## What Value Does a DoD Provide?

### 1. Software Quality

To developers, the most obvious impact of having a DoD is it serves as a tool to maintain the consistency and quality of the software being produced. A typical DoD will begin by addressing peer reviews and testing, which is complementary and orthogonal to acceptance criteria specific to a feature. Furthermore, a DoD may go beyond code, such as considering the deployment pipeline, cross browser/device support, security and performance.

### 2. Transparency & Predictability

From my experience in both leading a team or being a part of a team, some of the most underrated traits that people appreciate of their leadership or working environments are transparency and predictability. The DoD is a tool that helps in that regard, holding the team accountable while also protecting it from unreasonable or undocumented expectations. This is true whether the team is relatively self-organising or there is a more conventional top-down leadership style. Either way, I find the most effective DoDs have buy-in from the team because they have a role to play in shaping it.

### 3. Estimation

Beyond the advocacy of software quality and the social contracts of a team, a typically overlooked aspect of not having a clear DoD is the impact on estimation. The key input for the planning phase of a traditional project management approach is of course a breakdown of tasks with corresponding estimates of times to deliver each one. Yet without a clear understanding and consensus of what being _done_ means, not just within a delivery team but even including the wider business, how can an estimate and therefore a project plan be arrived at with any sort of accuracy?

As a case in point, on a recent project I was on where there was no clear DoD, a developer claimed he was "done" after 2 days on a task which was estimated at 4 days. However, in this case, not only was the code written in a manner which made it difficult to test, it also caused several regressions which became apparent as soon as the release made it into a test environment, long after it was declared "done". Imagine if this developer was the sole source of estimates for the project plan?

### 4. Communication

If you have been in the software delivery game for long enough, then chances are you will have sat through tedious meetings where the team will need to elaborate on the nuances of the status of every outstanding task to some sort of project management. Every. Single. Day. The typical developer response when there isn't a clear consensus of _done_ will begin with "done, but" ... only management aren't interested in the "but", move on and think for all intents and purposes that it's "done". Wouldn't it be great if status updates could be a relatively straight-forward _done_ or _not done_ affair, with elaboration only a necessity if a task was _not done_ and help was needed?

## What Should Be in a DoD?

Below are some potential considerations that could be incorporated into a DoD. What may make sense on a particular project will of course depend on the technology stack, the composition and maturity of the team, and the nature of the business itself.

- Does the feature fulfil requirements / acceptance criteria?
- Has the code been peer reviewed? Does it meet coding standards?
- What is the impact on the existing test suite? Are there any new tests?
- Has the code been merged into the appropriate branch?
- Is the build green? Has it been deployed to a CI environment?
- Is documentation appropriate?
- Is logging appropriate?
- Is security appropriate?
- Is performance appropriate?
- Has cross browser/device testing been performed?
- Does the feature meet UI/UX/accessibility requirements?
- Has the feature been tested by QA?
- Has the feature been demonstrated to or validated by the business?

## Final Thoughts

Remember, a DoD can be a living document! What typically happens is that as a team and its processes mature, a DoD can evolve with it.

## Links

 * [Scrum Guide](http://www.scrumguides.org/scrum-guide.html#artifact-transparency-done)
