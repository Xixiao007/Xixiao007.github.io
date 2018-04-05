---
title: "Extract custom objects from sandbox into scratch org"
layout: post
tags: Salesforce
comments: true
---

# Issue

There are many benefits of using [DX]({% post_url 2017-12-10-salesforce-dx-intro %}) to develop solutions for Salesforce, yet a big hurdle for envrionment that has historic custom metadata is that if DX solution has dependencies on any existing meta-data, such as custom objects, fields or configurations, we need to figure out how to extract them and save as DX format into the project so that DX project can be pushed into scratch org succesfully.

This is also a must-to-do action for environment to shift from traditional org centralized developing paradigm into version control centralized DX paramdigm.

In this article, we go through how to extract meta-data by SFDX commandline.

## Extract meta-data from sandbox or production

### Prerequisite

If we want to extract meta-data from sandbox, make sure it has up-to-date copy from production. Either create a new or refresh an existing sandbox is a good idea.

### Step 1 - package.xml

Prepare `package.xml` which comprises the immediate dependencies. For instance, we assume a custom sObjec with API name `System__c` is needed in DX project.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Package xmlns="http://soap.sforce.com/2006/04/metadata">
    <types>
        <members>System__c</members>
        <name>CustomObject</name>
    </types>
    <version>42.0</version>
</Package>
```

### Step 2 - retrieve meta-data

Retrieve `System__c` and all its custom fields from a sandbox named `sandbox1` via DX command.

Btw, DX can do all what Salesforce migration tool (ant tool) does, so there is no need to use migration tool anymore.

```shell
sfdx force:mdapi:retrieve -r retrieveCodeoutput -k package.xml -u sandbox1
```

And unzip retrieved data.

```shell
unzip retrieveCodeoutput/unpackaged.zip
```

### Step 3 - convert meta-data to DX format

Move the unziped folder into your DX project root path, if not done yet. This is because converting meta-data to DX format needs to be done in DX project root path.

Finally, converting.

```shell
sfdx force:mdapi:convert --rootdir unpackaged/
```

Above command will convert meta-data and save to a `object` folder.

### What next?

If we are lucky, `System__c` has no dependency on any other custom meta-data, run a DX push against a scratch org would succeed.

If not, the error from push action will give us a clue what extra meta-data we need to take into consideration too. In this case, go back to step 1 to include more meta-data and play the game again until DX push action goes without error.

# Conclusion

I had experience that there were multiple layer nested meta-data dependencies. `System__c` has lookup field on another custom object, listview is for a custom role, etc. It turned out to be mission impossible to achieve a self-contained DX solution goal.

In my opinion, any environment that wants to fully embrace DX paradigm needs to denote effort to extract all custom meta-data, saving to a dedicated DX package, and making sure all future meta-data are also saved into this package.

Once in DX, develping in dev sandbox should never ever be allowed. It is a black-or-white thing.