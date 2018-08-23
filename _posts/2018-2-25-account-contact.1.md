---
title: "Understand Accounts and Contacts in Salesforce"
layout: post
tags: Salesforce
comments: true
---

I am spending time to go through [CRM related trailheads](https://trailhead.salesforce.com/trails/getting_started_crm_basics?trailmix_creator_id=00550000006yDdKAAU&trailmix_id=prepare-for-your-salesforce-administrator-credential) in order to both brush up my CRM understanding, such as Accounts and Contacts, and prepare for the admin certificate.

If CRM terms is also one of your weak points, I'd recommend you walk through those trailheads with me. It will definitely give you a whole better picture of important CRM terms used in the Salesforce sales cloud.

In this post, we will study **accounts** and **contacts**. Take it for free and welcome!

# Accounts

**Accounts** in Salesforce refer to companies that we are doing business with, usually in the context of Business-to-Business(B2B) model.

If we are doing business with individual person, the so called Business-to-Customer(B2C) model, a special account type called **Personal Account** needs to be turned-on manually.

*Note. Person accounts are forever. Once they're turned on, they can't be turned off.*

# Contacts

**Contacts** is another important object in Sales Cloud.

In addition to which companies we are doing business with, we want to know is who are working in those companies and how to reach them out. In Salesforce, the people who work at the accounts are called Contacts.

# Relationship between Account and Contact

Real life scenarios defines the relationship possibilities. There are 3 scenarios are covered in Salesforce Account-Contact setup.

## Contacts to Multiple Accounts

One contact might work with more than one company. A series-entrepreneur owner might own more than one company.

*Note. One contact to multiple accounts needs to be enabled manually.*

## Account Hierarchies

A global company might have multiple branches in various geographic locations.

We can establish one global account and link all contacts, opportunities, cases, and so on to that single overarching account. 

- **Pro**: It is easy to find all the account's records and related information at one place.
- **Con**: It is difficult to manage a large voume of information and separate records by locations.

Salesforce officially recommends to establish accounts for each separate location. It is more flexibile in most of cases.

## Account Teams

More than one pserson works with each account is commonplace unless our company is teeny tiny. Such a team with multiple people comprises various roles, such as sales reps, sales managers, support agent, support managers etc.

An account Team in Salesforce can include up to 5 people, each of who can wear different hats with different level of access to the account and corresponding opportunies and cases.

*Note. Account team needs to be turned on and configured manually.*

# Social Accounts and Contacts

Believe it or not, social network has become an integral part of our lives. It is always a hate and love relationship. On one hand we enjoy the easiness it gives us to share and connect with others, on the other we hesitate and worry about if we have put too much personal information there.

In terms of business world, it may very well aid us in knowing better our customers by following up their social accounts. Salesforce out-of-box allows to add social network information from Twitter, Facebook, YouTube, and Klout to the records. Of course, we can always create custom fields to store information from other social networks.

*Note. social accounts and contacts is a feature that we need to manually enable in Salesforce.*