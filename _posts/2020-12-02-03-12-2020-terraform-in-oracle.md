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

**Note:** The presented tutorial here is based on the official tutorial from Oracle [Terraform: Set Up OCI Terraform](https://docs.cloud.oracle.com/en-us/iaas/developer-tutorials/tutorials/tf-provider/01-summary.htm)
{: .notice--info}

## Creating the Oracle Cloud Account

First of all you need to create an Oracle Cloud account. For this, you need to go to the [Signup page](https://signup.oraclecloud.com/) of Oracle. 
You will be required to enter a payment card in the profile for verification purposes. If you use only Always Free resources after the free trial has ended, you shouldn't worry about costs as they are as the name suggests, always free. In our exampe we will use the always free compute instance and the vcn network. 

Once you have finished following all the steps from the signup page, you will be waiting on the activation which might take from minutes to hours. While waiting on the activation, you can proceed with the next step

## Installing Terraform Locally

  You can install Terraform on Windows, Mac, or Linux. You can find the instalation instructions here: [Terraform Cli Instalation](https://learn.hashicorp.com/tutorials/terraform/install-cli)
  In my personal case, I use disposable virtual machines for these types of activities in order to keep my environment clean. For this tutorial I have used a CentOS 8 VirtualBox VM and used the following guide: [Official Oracle Terraform guide](https://docs.cloud.oracle.com/en-us/iaas/developer-tutorials/tutorials/tf-provider/01-summary.htm)


Download Terraform zip file:
```shell
https://releases.hashicorp.com/terraform/0.14.0/terraform_0.14.0_linux_amd64.zip
```

Unzip the file:
```shell
unzip terraform_0.14.0_linux_amd64.zip
```
Move the folder to `/usr/local/bin`
```shell
sudo mv terraform /usr/local/bin
```
Check the Terraform version:
```shell
terraform -v
```
After finishing installing Terraform, you need to wait for the activation of the Oracle account before continuing on the next step

## Setting Up Terraform For Oracle

### Create RSA Keys
Create RSA keys for API signing into your Oracle Cloud Infrastructure account.

Open a terminal window.
Make an .oci directory.
```shell
mkdir $HOME/.oci
```
Generate a 2048-bit private key in a PEM format:
```shell
openssl genrsa -out $HOME/.oci/<your-rsa-key-name>.pem 2048
```
Change permissions, so only you can read and write to the private key file:
```shell
chmod 600 $HOME/.oci/<your-rsa-key-name>.pem
```
Generate the public key:
```shell
openssl rsa -pubout -in $HOME/.oci/<your-rsa-key-name>.pem -out $HOME/.oci/<your-rsa-key-name>_public.pem
```
Copy the public key.
In the terminal, enter:
```shell
cat $HOME/.oci/<your-rsa-key-name>_public.pem
```
Add the public key to your Oracle user account.
From your user avatar, go to User Settings.
Click API Keys.
Click Add Public Key.
Select Paste Public Keys.
Paste value from previous step, including the lines with BEGIN PUBLIC KEY and END PUBLIC KEY
Click Add.
You have now set up the RSA keys to connect to your Oracle Cloud Infrastructure account.

### Gather Required Information
Prepare the information you need to authenticate your Terraform scripts and copy them into your notepad.

Collect the following credential information from the Oracle Cloud Infrastructure Console.

- Tenancy OCID:`<tenancy-ocid>`
From your user avatar, go to Tenancy:`<your-tenancy>` and copy OCID.
- User OCID: `<user-ocid>`
From your user avatar, go to User Settings and copy OCID.
- Fingerprint: `<fingerprint>`
From your user avatar, go to User Settings and click API Keys.
Copy the fingerprint associated with the RSA public key you made in section 2. The format is: xx:xx:xx...xx.
- Region: `<region-identifier>`
From the top navigation bar, find your region.
Find your region's `<region-identifier>`from Regions and Availability Domains. Example: us-ashburn-1.
Collect the following information from your environment.
- Private Key Path: `<rsa-private-key-path>`
Path to the RSA private key you made in section 2. Example: `$HOME/.oci/<your-rsa-key-name>.pem`.

### Create Scripts
In this section you create three scripts: one for authentication, one to fetch data from your account, and one to print outputs.

#### Add Authentication
In this section, you add API key based authentication. First, you set up a directory for all your Terraform scripts. Then you add a provider script so your Oracle Cloud Infrastructure account can authenticate the scripts running from this directory.

In your `$HOME` directory, create a directory called `tf-provider` and change to that directory.
```shell
mkdir tf-provider
cd tf-provider
```
Create a file called `provider.tf`.
Add the following code to `provider.tf`
Replace the fields with brackets, with the information you gathered in the previous step.
Make sure you add quotations around string values.
```terraform
provider "oci" {
  tenancy_ocid = "<tenancy-ocid>"
  user_ocid = "<user-ocid>" 
  private_key_path = "<rsa-private-key-path>"
  fingerprint = "<fingerprint>"
  region = "<region-identifier>"
}
```
Save the `provider.tf` file.

#### Add a Data Source

In this section, you fetch a list of the availability domains in your tenancy.

In the `tf-provider` directory, create a file called `availability-domains.tf`.
Add the following code to `availability-domains.tf`.
Replace the field with brackets, with the information you gathered in previous section.

Source from <https://registry.terraform.io/providers/hashicorp/oci/latest/docs/data-sources/identity_availability_domains>
{: .notice--info}

`<tenancy-ocid>` is the compartment OCID for the root compartment.
Use `<tenancy-ocid>` for the compartment OCID.
```terraform
data "oci_identity_availability_domains" "ads" {
  compartment_id = "<tenancy-ocid>"
}
```

Make sure provider.tf and availability-domains.tf are located in the same directory. Terraform can process all the files in a directory in their correct order, based on their relationship. Therefore, separate the provider and your other scripts for a modular approach and future reuse.

