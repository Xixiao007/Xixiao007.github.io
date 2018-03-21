---
title: "Prototyping Lightning Component, SFDX, LTS and Travis[1]"
layout: post
tags: developer
comments: true
---

I have built a prototyping project---and saved in <a href="https://github.com/Xixiao007/sfdx-tryout" rel="noopener" target="_blank">Github repo</a>---to demonstrate how to:

- Create Lightning Components
- Unit testing Lightning Components
- Use SFDX to scaffold project structure
- Wire everything up in Travis---A SaaS Continuous Integration service

In this post series---2 articles in a row, I am gonna use this project to break these knowledge points down and dig into them. This blog post is the first article of the two.

**What we'll do in this article:**

- Go through the mindmap for the prototyping project (most important!)
- Study recommended trailheads to learn Lightning Component
- Organize the project with SFDX style
- Collect some dummy user requirements and implement them
- List down the remaning topics for article 2

# Mindmap

The source code stored in my prototyping project is merely the final version of the work. It is more intriguing and useful to understand how I reached this final version. Thus a mindmap below tells the story.

![mindmap]({{ "/assets/img/mindmap.png" | absolute_url }})
<!-- <img src="http://www.salesforceway.com/wp-content/uploads/2018/01/mindmap-1024x561.png" alt="mindmap" width="680" height="373" class="alignnone size-large wp-image-178" /> -->

## Content in article 1

1. the project starts from this trailhead project: <a href="https://trailhead.salesforce.com/projects/workshop-lightning-restaurant-locator" rel="noopener" target="_blank">Build a Restaurant-Locator Lightning Component</a>. It is the first blue box in the top-left corner.

2. I collected several piece of "user voice" from nearby colleagues by asking them "what would you like to have if you were the user of this locator app?" and implemented them with SFDX stle. They are blue box 2 and 3 in the flow.

## Content in article 2

1. I create two dummy Lightning Component unit tests in LTS and wired everything up in Travis, a Saas Continous Integration service.

2. I'll make a summary about pros and cons from my standing point of view.

Are you ready? let's get the ball rolling now!

# Study base project

Before going through my prototyping project, make sure you have done this trailhead project <a href="https://trailhead.salesforce.com/projects/workshop-lightning-restaurant-locator" rel="noopener" target="_blank">Build a Restaurant-Locator Lightning Component</a>.

This trailhead project serves as a baseline for the following activities.

# Collect user feedback

The trailhead project above is done in the Developer Console. Developer Console is good for quick and dirty test, rather than real life projects.

In this step, I started by re-creating all files by SFDX commands, such as create project, create lightning component file etc. What is SFDX? Check this <a href="http://www.salesforceway.com/2017/12/sfdx-intro/" rel="noopener" target="_blank">blog thread</a>

By doing so, the project file structure is re-arranged according to SFDX style. What's more, using SFDX helps us to automate the deploying process and integrate to any Continous Integration system. This benefit will not be visible in steps of this article but the subsequent one where Continous Integration is discused.

Then I collected "user requirements" from fake users--i.e. colleagues close by. These are gathered requirements:

> - "I definitely need a map for quick action!"
> - "Clicking to show/hide details (in the restaurant in the list)"
> - "List is OK, but data visualization is better"
> - "I need Only high rated restaurants, and sorted for me"
> - "Mobile first!"

# Implementation

Then these requirements are implemented in the prototying project. The information Architect diagram depicts the high level picture and data flow among components.

![info-arch]({{ "/assets/img/info-arch.png" | absolute_url }})

In order to see the details, refer to its code in <a href="https://github.com/Xixiao007/sfdx-tryout" rel="noopener" target="_blank">Github repo</a>, I also strongly suggest to install it into a scratch org---check the repo README file.

# Article 2

In the following article, we'll do:

- Write Lightning Component unit tests
- Put everything into a Continuous Integration pipeline

Stay tuned!
