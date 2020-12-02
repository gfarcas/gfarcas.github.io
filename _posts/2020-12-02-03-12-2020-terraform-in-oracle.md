---
title: Provisioning Compute with Terraform in Oracle Cloud
date: 2020-12-05T19:34:30-04:00
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

Once you have finished following all the steps from the signup page, you will be waiting on the activation which might take from minutes to hours. While waiting on the activation, you can proceed with the next step

## Installing terraform locally

  You can install Terraform on Windows, Mac, or Linux. You can find the instalation instructions here: [Terraform Cli Instalation](https://learn.hashicorp.com/tutorials/terraform/install-cli)
  In my personal case, I use disposable virtual machines for these types of activities in order to keep my environment clean. For this tutorial I have used a CentOS 8 VirtualBox VM and the following commands installed Terraform succesfully. 

Install yum-config-manager to manage your repositories.

`sudo yum install -y yum-utils`

Use yum-config-manager to add the official HashiCorp Linux repository.

`sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo`

Install.

`sudo yum -y install terraform`