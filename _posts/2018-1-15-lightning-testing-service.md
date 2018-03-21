---
title: "Lightning Testing Service"
layout: post
tags: developer
comments: true
---

> The Lightning Testing Service, or LTS, is a set of tools and services that let you create test suites for your Lightning components.

Today we are gonna take a look at it by accomplishing 2 things:

1. Installing LTS and running the by-default test suites
2. Study LTS source code structure and key file content to understand how it is wired up under the hood

Despite `.cmp` is a xml-style file, other Lighting Component (LC) files are Javascript and CSS---front-end stack. 
Therefore testing LC is really about testing JavaScript logic. Popular and well-maintained JavaScript test frameworks exist in the open source world, such as [Jasmine](https://jasmine.github.io){:target="_blank"} and [Mocha](https://mochajs.org/){:target="_blank"}.

There is really no need for us to reinvent the wheel, isn't it? Actually after going through this thread, we'll see that LTS is indeed merely a wrapper sitting on top of Jasmine and Mocha. 

Let's start the journey.

# Install and run sample tests

Human are visual creatures, we memorize better by seeing things moving. 

## Installation

LTS works the best with SFDX. If you are not familiar with SFDX, check my [SFDX Intro]({% post_url 2017-12-10-salesforce-dx-intro %}) and [SFDX Cheat Sheet]({% post_url 2017-12-25-sfdx-cheatsheet %}) blog threads.

Until this stage, I assume you have gone through the above two threads with these prerequisites done:

1. SFDX is succesfully installed
2. DevHub is connected
3. A brand new scratch org is created

Not ready? Go back to read my thread and get them done.

Now, we can proceed to install it.

```shell
sfdx force:lightning:test:install
```

What does this command do? It uploads related files into `Static Resources`. As simple as this!

If you open the scratch org, go to `setup` -> `Static Resources`, you will see those files uploaded.


## Run test suites

Then let's run the test suite coming in the LTS installation. Note. `-o` parameter keeps the running-tests browser open.

```shell
sfdx force:lightning:test:run -a jasmineTests.app -o
```

We should see a browser opens for us, with title of `18 specs, 0 failures, 1 pending spec` and bunch of trivial details. It means 17 out of 18 tests passed and the last one is suspended on purpose.

That's all for this topic, bye!
LOL, just kidding. Let's dig into the rabbit hole as step 2.

# How LTS is wired up

For the purpose of convenience, let's clone the source code to local and check it.

```shell
git clone https://github.com/forcedotcom/LightningTestingService.git
```

`lightning-component-tests` folder in the cloned repo is the core part. It comprises two subfolders:

1. `main/` - a sample Lightning App
2. `test/` - contains both static files used to initiate and run Jasmine and Mocha frameworks and test suites running against the app in `main/`.

How about things inside `test/` folder? I found 3 points worth mentioning:

1. `default/aura/staticresources/` - all out-of-box test suite files
2. `default/aura/jasmineTests/jasminTests.app` - as the content below, it is a Lightning App that uses `lts_jasmineRunner` to run 3 tests suite located in `aura/staticresources/` folder. It calls to `c:lts_jasmineRunner`---a custom Lightning Component.

```html
<aura:application>
    <c:lts_jasmineRunner testFiles="{!join(',', 
    	$Resource.jasmineHelloWorldTests,
		$Resource.jasmineExampleTests,
		$Resource.jasmineLightningDataServiceTests
    )}" />
</aura:application>
```

3. `aura/lts_jasmineRunner` - the very component called by `jasminTests.app`. Let's paste the content of `its_jasmineRunner.cmp` below. We see that `<ltng:require>` component serves the main course in 4 steps below:

- load `jasmine.js` and `jasmine-html.js` from Jasmine framework
- does the initialization in `lts_jasmineboot` 
- syntax sugar in `lts_testutil`
- run a `runTests` after-trigger at the end

```html
<aura:component extensible="true">
    <aura:attribute name="testFiles" type="String[]" required="true" description="list of test spec files"/>

    <!--  placeholder div which example test specs use to render components under test -->
    <div aura:id="renderTestComponents" id="renderTestComponents"></div>

    <!-- Placeholder div for jasmine test results  -->
    <div id="jasmineHtmlTestResults"/>

    <!-- Pull in jasmine, utils and the test specs specified by the consuming wrapper test app  -->
    <ltng:require scripts="{!join(',', 
        $Resource.lts_jasmine + '/lib/jasmine-2.6.1/jasmine.js',
        $Resource.lts_jasmine + '/lib/jasmine-2.6.1/jasmine-html.js',
        $Resource.lts_jasmineboot,
        $Resource.lts_testutil,
        v.testFiles
    )}"
        styles="{!join(',', 
        $Resource.lts_jasmine + '/lib/jasmine-2.6.1/jasmine.css'
    )}"
        afterScriptsLoaded="{!c.runTests}" />
</aura:component>
```

# Summary

In this article, we have done two things:

1. Installing LTS and running the out-of-box test suites in order to gain a basci idea how LTS works. 
2. Investiating how LTS calls Jasmine to run tests.

# To-Do list

In order to fully understand how LTS works, two parts remain to be revealed:

- How Lighting Component keywords are exposed to Jasmine?
- The content of `lts_jasmineboot.resources`. It performs all of the necessary initialization.