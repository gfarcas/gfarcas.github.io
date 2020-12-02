---
title: Provisioning compute with Terraform in Oracle Cloud
date: 2020-12-05T15:34:30-04:00
categories:
  - Blog
author: George Farcas
tags: [oracle, oci ,terraform]
summary: [A simple guide for provisioning infrastructure with Terraform in Oracle Cloud Infrastructure]
toc: true
toc_label: "Content"
---

One of the domains where automation can save a lot of time is infrastructure provisioning. In many cases, manualy provisioning infrastructure can take hours and it is prone to user error on each iteration. Here is where Terraform from HashiCorp comes handy. You can create, manage, and destroy the resources from the terraform code. Terraform has countless provider API integrations as well as modules where several resources are used toghether.

You might ask, why I have chosen Oracle Cloud for this tutorial? Well, I have done that for the following reason:
- Oracle Cloud Free Tier offers (at the time of the article) 2 Compute virtual machines with 1/8 OCPU and 1 GB memory each.
Even though AWS and Azure have always free tiers, they do not offer compute resources for free. Google offers a micro instance and I will make a tutorial covering that too.

## Creating the Oracle Cloud account

First of all you need to create an Oracle Cloud account. For this, you need to go to the [Signup page](https://signup.oraclecloud.com/) of Oracle. 
You will be required to enter a payment card in the profile for verification purposes. If you use only Always Free resources after the free trial has ended, you shouldn't worry about costs as they are as the name suggests, always free. In our exampe we will use the always free compute instance and the vcn network. 

Once you have finished following all the steps from 

