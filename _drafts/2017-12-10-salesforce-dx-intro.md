---
title: "Salesforce DX Introduction"
layout: post
tags: developer
comments: true
---

Hello world! I build up this sample case to introduce Salesforce Development Experience, namely `Salesforce DX`, to beginners. Hopefully it is as useful for you as it was for me.

In case you are interested in, I also created a <a href="https://gist.github.com/Xixiao007/e22aad45caf67df33aeafae085810570">Cheat Sheet</a> to summarize the most common commands for a quick reference.

# Learning Objectives

In this article, you will:

- Create a Dev Hub Trial account
- Install SFDX Tool
- Spin up a scratch org
- Add a custom field
- Pull and save meta-data changes to local environment
- Push changes to a new scratch org
- Get a list of Trailheads for further study
- Thinking ahead what SFDX can do better

The source code in this article is stored in [Github](https://github.com/Xixiao007/sfdx-demo-project).

# What is SFDX

In my definition, Salesforce Development Experience (SFDX) is the new tool to allow local development environment---instead of Salesforce org---to act as the only source of truth.

What does it mean? It means any change of org meta-data, Apex, Lightning Component, or Visualforce can be totally decoupled from any org environment and 100% saved in the local environment!

Is this a good thing? Of course it is, buddy! With the help of SFDX, Salesforce development experience shifts from dealing painfully with a big slow complex monolithic monster to dancing joyfully with a agile flexible modularized real frond-end like beauty.

Let's Learn it together by going through a sample project.

# Register a Dev Hub

Yeah, we start the journey from scratch here! First thing first, let's register a Dev Hub trial account.

For learning purpose in this article, I strongly recommend to sign up a Dev Hub 30-day trial account [here](https://developer.salesforce.com/promotions/orgs/dx-signup).

Once you know what you are doing, you can then enable and use the Dev Hub in your production environment by following [here](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_enable_devhub.htm).

# Installing SFDX

Assuming your Dev Hub account is created, we now install SFDX.
You can install SFDX by following [this official instruction](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli.htm).
I personally prefer installing via *NPM* by [this instruction](https://developer.salesforce.com/docs/atlas.en-us.sfdx_setup.meta/sfdx_setup/sfdx_setup_install_cli_npm.htm). The reason, I need *node* for other tasks and installing via *NPM* works across platforms and goes like a charm.

After SFDX is installed, remember to use `sfdx --version` to validate the installation. No error, right? Cool! Move on!
{% highlight bash linenos %}
$ sfdx --version
sfdx-cli/6.0.15 (windows-x64) node-v8.9.1
{% endhighlight %}


## Play with Sample project

We are ready, let's get the rubber hit the road now.

### Log into Dev Hub

With the Dev Hub account at hand, we need to link the local environment with it by running this command:
<pre class="theme:github lang:shell decode:true ">sfdx force:auth:web:login -d -a DevHub</pre>
A login tab will pop up in the browser and we can sign in with our new lovely Dev Hub account.

**Hint. if you are not sure about any SFDX command or what parameters it accepts, *-h* is the secrete tool to turn to**, e.g.
<pre class="theme:github lang:shell decode:true ">sfdx force:auth:web:login -h</pre>
To verify the success of Dev Hub log-in, run the following command and you should see your Dev Hub information, like username and connected status:
<pre class="theme:github lang:shell decode:true ">$ sfdx force:org:list
=== Orgs
     ALIAS   USERNAME              ORG ID              CONNECTED STATUS
───  ──────  ────────────────────  ──────────────────  ────────────────
(D)  DevHub  xi.xiao001@gmail.com  00D1I000003GUIuUAO  Connected
</pre>
### Create a local project

Here we scaffolded a local project with folder named *MyProject* in the current path.
<pre class="theme:github lang:shell decode:true ">sfdx force:project:create -n MyProject</pre>
Feel free to go through the *MyProject* folder structure to see what files are automatically created for us.

### Create and open a scratch org

Firstly, make sure you are in the *MyProject* project root path. Then we start the fun part---initiate a new *scratch org* under your Dev Hub.

Scratch org is an exciting new thing in Salesforce---I know, not so new in front-end world, but still. It is, under its hood, a container---what *container* is, check [docker](https://www.docker.com/)---encapsulating a clean dev org.
The best thing about *scratch org* is that it can be span up or torn down really quickly, say within 20 seconds! Imagine we initiate a scratch org, do some testing, and terminate it within just a minute or so. How cool is that!?

Here are the commands to spin up a *scratch org* with the name *tempTest*. Again, remember to use **-h** following the command to check the meanings of those parameters.
<pre class="theme:github lang:shell decode:true ">sfdx force:org:create -s -f config/project-scratch-def.json -a tempTest</pre>
And we of course can interact with the scratch org via browser.
<pre class="theme:github lang:shell decode:true">sfdx force:org:open</pre>
Be noted that the above opened scratch org has a random admin user generated and automatically signed in for you!

Hint. using *sfdx force:org:list* to check your Dev Hub and scratch orgs status at any given time.

### Interact with the scratch org

#### Create a custom field

Once the scratch org is opened in the browser, you should see lightning experience already enabled for you.

Let's do some point-and-click to add a custom field in Account Object. You don't have to follow the exact steps. As long as a new custom field is added, you are good to go. For example, what I did:

- Go to *Setup*
- Click in *Object Manager* on the left panel
- Click *Account*, then *Fields &amp; Relationships*
- Click *New* to create a new field with *Text* Data Type
- Fill in *Descpt* for the *Field Label*, 10 for the *Length*
- Click *next* until the new custom field is created

#### Check the changes

Now using *sfdx force:source:status* to check the difference between local and scratch org. See the detected changes?
<pre class="theme:github lang:shell decode:true ">$ sfdx force:source:status
=== Source Status
STATE FULL NAME TYPE PROJECT PATH
────────── ────────────────────────────────── ─────────── ────────────
Remote Add Account.Note__c CustomField
Remote Add Admin Profile
Remote Add Custom: Sales Profile Profile
Remote Add Custom: Marketing Profile Profile
Remote Add Custom: Support Profile Profile
Remote Add Account-Account (Marketing) Layout Layout
Remote Add Account-Account (Sales) Layout Layout
Remote Add Account-Account (Support) Layout Layout
Remote Add Account-Account Layout Layout
</pre>
#### Pull changes to local

As we said, the local environment is the only source of truth, rather than the scratch org. We wanna pull the changes down to our local folder by:
<pre class="theme:github lang:shell decode:true ">sfdx fource:source:pull</pre>
Once the command runs succesfully, you can find the meta-data is saved under the path *force-app\main\default\objects\Account\fields*

#### Push the change to another scratch org

The *new custom field creation* is saved in the local project, let's delete the current scratch org, spin up a new one and push the change to it just for the sake of testing. Without much explanation, you should know what these commands do :).
<pre class="theme:github lang:shell decode:true ">sfdx force:org:delete -u tempTest -p
sfdx force:org:create -s -f config/project-scratch-def.json -a tempTest2
sfdx force:source:push
sfdx force:org:open
</pre>
In the new opened scratch org, let's validate that the custom field is automatically added into Account object. If so, congratulation! You have just completed all steps in this artile!

## Further Reading

Listen buddy, this is just the tip of iceburg. SFDX can do much more, such as push local code, be integral in the DevOps pipeline and so on.
To gain a holistic view, I'd recommend that you pick up [these trailheads](https://trailhead.salesforce.com/trails/sfdx_get_started) on your interests.

## Thinking outside the box

&gt;If all you have is a hammer, everything looks like a nail.

Get acquainted to a tool is NEVER my ultimate objective of learning since countless "best practices" in the internet exist. My goal is to understand how the learned thing differs from other knowledge we know, its pros and cons and more. With such, I am capable of determining when and when not to use the tool/knowledge.

### SFDX syntax issue
IMHO, the command syntax is not beginner friendly. For instance, to pull changes, it is *sfdx force:org:pull*. To drawing analogy, what does *git* do to pull data from remote? Well, as simple as *git pull*. No one is supposed to remember any of those chained namespaces, like  *force:org:pull*.

Thus I created a [Cheat Sheet](https://github.com/Xixiao007/sfdx-cheatsheet) in Github for referring purpose. Alternatively we can also create shell alias to save our typing and thinking :).

### Slow feedback issue

Despite that scratch org is a big improvement, an interaction between local environment and scratch org still takes 3-5 seconds. Coming from front-end development world with features like hot load, auto refresh, local container, etc., I consider the feedback loop is **too slow**, simply a buzzkill.

Since Salesforce is a proprietary service, we might never get to use a local container. However, for Lighting Component development, we can build aura components in a jetty-installed docker image, and use scratch org to host and render as step 2 when Lightning Component features are to be developed.

### Your turn

So I have share both "good" parts and "bad" parts in my mind. What do you say? If you were designing SFDX and its command line, what would you have done differently and better?

**Share your thoughts in the commenting area and I will add those nice ones here.**

Oh, don't forget to subscribe :)!