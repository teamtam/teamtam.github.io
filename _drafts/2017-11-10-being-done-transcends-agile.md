---
title: Being Done Transcends Agile
description:  "Why being \"Done\" should not just be an agile thing"
author: Benjamin Tam
categories: Agile
tags: [Scrum, Done]
comments: true
---

While the words _agile_ or _scrum_ are now part of the vernacular of the software industry, the reality is many organisations are somewhere on the spectrum between it and a more traditional approach. Some purists may even view _agile_ and _waterfall_ as being binary and mutually exclusive. Regardless, if there is one idea that I think should transcend ideology is that of the _Definition of Done_ (DoD).

I remember my first experience as a team leader many years ago before agile became the defacto standard and I tried to steer the ship towards agility. I had a team who had learnt by gradual osmosis what my expectations were when completing a feature. However, when a new team member joined the fray, he did not appreciate my feedback that he wasn't actually done. In hindsight, a DoD would have obviously helped mitigate or even prevent a difficult conversation.

## So What Value Does a DoD Provide?

1. Transparency & Predictability
From my experience at both leading a team or being a part of a team, one of the most underrated traits that people appreciate of their leadership or working environments are transparency and predictability. The DoD is a tool that helps in that regard, holds the team accountable while also protecting it from unreasonable or undocumented expectations. This is true whether the team is relatively self-organising or there is a more conventional top-down leadership style. That being said, I find the most effective DoD have buy-in from the team because they have a role to play in shaping it.

2. Software Quality
To developers, the most obvious impact of having a DoD is it serves as a tool to maintain the consistency and quality of the software being produced. A typical DoD with begin by addressing peer reviews and testing, which is complementary to and orthoganal to acceptance criteria specific to a feature. Furthermore, a DoD may go beyond code, such as considering the deployment pipeline, cross browser/device support, security and performance.

3. Estimation
Beyond the _touchy-feelies_ of the team and the often intangible measures of software quality, a typically overlooked aspect of not having a clear DoD is the impact on estimation. I find this ironic, in that businesses with traditional  project management often come hand-in-hand with strong expectations of fixed scope, time and/or budget. The key input for the planning of this approach is of course estimation. Yet without a clear understanding and consensus of what being _done_ means, not just within a delivery team but even including project management, how can an estimate be arrived at with any sort of accuracy?

As a case in point, on a recent project I was on where there was no clear DoD, a developer claimed he was _done_ after 2 days on a task which was estimated at 4 days. However, in this case, not only was the code written in a manner which made it difficult to unit test, it also caused a number of regressions which became apparent as soon as the release made it into a test environment, long after it was declared _done_. Imagine if this developer was the source of estimates on the project plan?

4. Communication
If you have been in the software delivery game for long enough, then chances are you will have sat through tedious meetings where the team will need elaborate on the nuances of the status of every outstanding task. Every day. And the typical developer response begins with "done, but" ... only management aren't interested in the "but" and move on. Wouldn't it be great if status updates could be a relatively straight-forward _done_ or _not done_ affair when they are a necessity, with elaboration only required if a task was _not done_ and help was needed too?

## What Should Be In a DoD?

Considerations...

- Fulfil requirements / acceptance criteria
- Peer review
- Tests
- Branching
- CI
- Cross browser/devices, depending on the nature of the project
- Logging
- Security
- Deployment
- Performance

(flesh out, examples)
(tooling?)

## Final Thoughts

Remember, a DoD can be a living document! What typically happens is that as a team and its processes mature, a DoD can evolve with it.
