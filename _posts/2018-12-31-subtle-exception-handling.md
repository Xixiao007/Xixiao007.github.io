---
title: "Exception throwing test in Apex"
layout: post
tags: salesforce
comments: true
---

# Unit Test - Exception throwing

One of edge cases to unit testing Apex code is the "exception throwing" scenarios, like below:

```java
try{

} catch (Exception e){
  throw new CustomException('my own message');
}
```

In order to enter the `catch` and `throw` lines we need to create unit test that does reach an exception stage and allow the code to throw different exception. I had no issues to construct these tests most of time, but recently a subtle case educated me.

In short, the unit test I constrcuted reached an exception that is different from what I expected and it still passed the unit test. David Reed in the thread below gave a very good tip -- assert test the error message in addtion to all what I usually do.

The whole story is recorded in this question thread in Stack Overflow [here](https://salesforce.stackexchange.com/questions/244800/code-coverage-not-cover-fully-cover-block-in-unit-test/244801#244801)
