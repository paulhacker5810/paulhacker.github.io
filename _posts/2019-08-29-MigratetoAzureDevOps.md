---
layout: post
title: Migrating to Azure DevOps
---

I have been working with a lot of companies migrating from Team Foundation Server(TFS) or Azure DevOps Server to Azure DevOps in the cloud.
What I am finding is that, while there is a migration guide, they either fail to read it or follow it. Then they have a botched migration and they blame Microsoft for the issues. The migration guide does a good job if you follow the directions. In this post I want to go over the steps and help people understand how to do a migration. Again there is a guide for this people, but I'll share some lessons learned.

First off, as of this post, the migration tool only works with the previous two versions of Azure DevOps Server. So right now that is Azure DevOps Server 2019 or 2019.01

If you are still on a version that goes back more than two versions, you have two choices. Upgrade your production TFS/Azure DevOps Server or stand up an Azure DevOps Server "jumpbox" to use for the migration. That can be a VM in Azure, or a VM in your datacenter. Either way you need to install Azure DevOps Server and SQL Server.

So lets start with the Upgraded scenario. If you upgraded, or have Azure DevOps Server running, its time to start the work. Which I will outline below.

If you are using the "jumpbox" method, there are a couple extra steps you need to do. 

First off you need to stop the collection on your production environment and then detach the collection (Yes there is some down time). Now that its detached, back it up and move the .bak file to the Azure DevOps Server you stood up. Once that database is backed up, reattach and get your production instance back online as soon as possible.

Once its uploaded or moved over, you need to restore it to SQL Server. Once that is done attach the collection. It will take a bit to attach since it has to configure things for the latest version.

Once the attach is done and the collection is online, go make sure it all looks like it should.

Now with that done, the work begins. It does not matter what method you used to get to the latest version of Azure DevOps Server, the following steps are the same.

First off the migration guide is located here so I strongly suggest you download it.
https://www.microsoft.com/en-us/download/details.aspx?id=54274 

First off you need to stop the collection on your production environment and then detach the collection (Yes there is some down time). Now that its detached, back it up and move the .bak file to the Azure DevOps Server you stood up.
Once its uploaded or moved over, you need to restore it to SQL Server. One that is done attach the collection. It will take a bit to attach since it has to configure things for the latest version.

Once the attach is done and the collection is online, go make sure it all looks like it should.

Now that that is done, the work begins. It dos not matter what method you used to get to the latest version of Azure DevOps Server, the following steps are the same.


First off the migration guide is located here so I strongly suggest you download it.
[Azure DevOps Migration Guide ](https://www.microsoft.com/en-us/download/details.aspx?id=54274 )

Now lets go through the 5 sections of the guide.

**Section 1 - Get Started** 

It's just an overview as well as the link to the migration tool. Which is the same link that is above for the guide. Depending on the version of Azure DevOps Server you are on, there is a tool for it. So right now there is the Migrator for 2019RTW and 2019.01.

**Section 2 - Cloud Pre-requisites**

What this is focused on is the fact that you have setup Azure Active Directory and have it replicating with your on premises AD.

**Section 3 - Upgrade** 

This section is all about the options to upgrade to the latest version. I have already outlined a couple options, but if you are on an much older version of TFS, you will want to review this section to get you upgraded correctly. 

**Section 4 - Validate** 

This section is focused on the validation step. At this point you will run the migrator from a command prompt or Powershell window. The command you are going to run is similar to this:
Migrator validate /collection:http://localhost:8080/tfs/DefaultCollection 
Just replace the values with what matches your environment. This will generate a validation report. You have to get a successful validation to move on. if you have a modified process template, this is where there may be some issue. Not all your customizations may move over cleanly. This means you have to back out those changes and try again. Once the migration is complete you have the ability to make the changes in Azure DevOps. Remember the goal here is to validate sucessfully.

**Section 5 - Prepare** 

This section is focused on preparing for import.
**NOTE:** ***Many legacy subscribers will find that their subscription was assigned to and activated
with a Microsoft Organization. The last step for each subscriber to take is to link the
Visual Studio Subscription to their Azure Active Directory organization. There are a
few different methods for doing this step which are documented at https://aka.ms/LinkVSSubscriptionToAADOrganization***

Now you will need to run the prepare step. This is going to generate the files needed for the migrator to import correctly. The two files you really care about are the Identities.csv and the import.json. The identities file lists all the users and there status, either active or historical. You want to be sure all accounts that are currently active are listed as Active. If they are listed as Historical, that means its an account that was not matched in AAD to a user. 
NOTE: If you migrate a user in as Historical, you CANNOT move it back to Active. That means you need to re-add the user into Azure DevOps. This is important, it will break the linkage to their work that's alrady in the system. so while names are the same, they wont match up to their work.