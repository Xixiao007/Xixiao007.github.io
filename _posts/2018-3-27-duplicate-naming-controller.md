---
title: "Issue: controller functions with the same name"
layout: post
tags: developer, lightning component
comments: true
---
## Lesson Learned

TLTR.
>Never use the same name for functions on both client side JavaScript controller and server side Apex controller.

## Details

I created a Lightning Component (LC) to start with very simple functions as below:

1. A button triggers a client side controller function named `submit`.
1. `submit` function makes a AJAX call to server side Apex controller function, which is with the same name `submit`.
1. `Console.log` the result in the browser.

### The code for the button

```html
  <lightning:button label="Submit!" onclick="{!c.submit}" />
```

### The code for the client side controller

```javascript
  submit: function (cmp, event, helper) {
    var action = cmp.get("c.submit");
    action.setParams({});
    action.setCallback({
      this, function(response) {
        var state = response.getReturnValue();
        if (state === "SUCCESS") {
          console.log("result: " + result);
        }
      }
    });
    $A.enqueueAction(action);
    console.log("ajax fired");
  }
}
```

### The code for server side Apex controller

```java
  @AuraEnabled
  public static String submit(){
    return 'echo hello';
  }
```

### The issue

The code was deployed to scratch org and I was so sure it would run like a charm.

However, after the button was clicked, the AJAX call got invoked infinitely and the `result` String returned from server was `undefined`.

I originally thought that the AJAX call was nested somewhere in the code so that it calles itself madly. Then I removed this line in client side `submit` function:

```javascript
$A.enqueueAction(action);
``` 

Then it worked as expected. When the button is clicked, both `ajax fired` and `echo hello` are printed out in sequence only once.

Here comes the weired part...

I really couldn't figure out why on earth the issue appears and I compared my `action` AJAX call with the sample from Salesforce document, they are nearly identical. 

I then pulled out yellow rubber duck, yet no positive result.

After a hour or two stupid debugging, all of a sudden, I recalled that it was mentioned somewhere **client side and server side controller function naming should be different**. 

Is this the root cause!? Yes, it is!

### Thoughts

Hopefully I won't make this mistake any longer.

Nonetheless, Synx checking or compiler should have complained, or at least warned, instead of relying on developers to do manual checking.