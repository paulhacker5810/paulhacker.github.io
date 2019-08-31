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
First off you need to stop the collection on your production environment and then detach the collection (Yes there is some down time). Now that its detached, back it up and move the .bak file to the Azure DevOps Server you stood up.
Once its uploaded or moved over, you need to restore it to SQL Server. One that is done attach the collection. It will take a bit to attach since it has to configure things for the latest version.

Once the attach is done and the collection is online, go make sure it all looks like it should.

Now that that is done, the work begins. It dos not matter what method you used to get to the latest version of Azure DevOps Server, the following steps are the same.


First off the migration guide is located here so I strongly suggest you download it.
https://www.microsoft.com/en-us/download/details.aspx?id=54274 

Now lets go through the 5 sections of the guide.

Step 1 - Get Started 
Its just an overview as well as the link to the migration tool. Which is the same link that is above for the guide. Depending on the version of Azure DevOps Server you are on, there is a tool for it.
