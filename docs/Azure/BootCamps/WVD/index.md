# Technical Intensity Workshop - Windows Virtual Desktop

Here are the lab files for the Windows Virtual Desktop TIW:

## Homework Assignment

In order to maximize our time together you have homework to complete prior to starting the WVD labs.  

Complete this [Hybrid Identity Lab](wvdhomework.md) prior to attending the WVD Workshop.  This lab will take approximately an hour to complete.

## Lab 1 - Hybrid Identity

In lab 1 you are going to establish ground to cloud connectivity using vNet peering and then build out a domain controller in the cloud that will sync to Active Directory on the ground.

* [Windows Virtual Desktop - Lab 1 - Identity](wvdlab01.md)

## Lab 2 - WVD Core Infrastructure

Creating a tenant in Windows Virtual Desktop is the first step toward building your desktop virtualization solution. A tenant is a group of one or more host pools. Each host pool consists of multiple session hosts, running as virtual machines in Azure and registered to the Windows Virtual Desktop service. Each host pool also consists of one or more app groups that are used to publish remote desktop and remote application resources to users. With a tenant, you can build host pools, create app groups, assign users, and make connections through the service.

In this lab you will learn how to:

> * Grant Azure Active Directory permissions to the Windows Virtual Desktop service.
> * Assign the TenantCreator application role to a user in your Azure Active Directory tenant.
> * Create a Windows Virtual Desktop tenant.

* [Windows Virtual Desktop - Lab 2 - Core Infrastructure](wvdlab02.md)
