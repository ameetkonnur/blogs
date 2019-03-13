---
title: azure usage & cost management reports with budgets & alerts
---

## what

For cloud users, Governance & Billing are two important aspects that overwhelm them. While one can take proactive measures to ensure certain policies and process are implemented, over time these dont seem enough as quickly there is sprawl of resources. Often too much control comes at a cost of agility and innovation. To keep a balance, you need to be less controlling but keep a tab on how resources are being deployed and how much does it cost. 

Currently Azure Portal doesnt give an integrated view of all resources & their costs in a simple and easy way. This project attempts to address that with a few features like self service BI, cost budgets and email alerts.

![sample dashboard](https://github.com/ameetkonnur/blogs/raw/master/img/billing-1.gif)

#### note

What this solution is not is a replacement for your Azure Portal. In time the Azure Portal will start offering these features in much more consolidated and richer way. Till such time this solution will address small gaps that exists in the Azure Portal.

## why

- it gives you a single view into your resources, costs, budgets
- since the data is your control, you can slice and dice it the way you want to
- it can give you a timeline on how your resources have grown
- long term data retention on billing & resources
- customized budgets and alert policies

## how

This solution uses two data sources provided by Azure.

- Azure EA Billing API (this gets all billing data for an enrollment)
- Azure Resource Graph API (this provides information for azure resources)

It takes data from the above two sources and stores them in custom mySQL database. These data pull jobs have been written in Python.
Once the data is in mySQL database, we have a set of pre-configured reports using apache superset (<https://superset.incubator.apache.org/>) which is a cool open source visualization tool.

The python jobs and apache superset are shipped in a container image that can be run anywhere. For the mySQL database, you can run it anywhere. Specifically for Azure deployment, i would recommend Azure Container Instance & Azure Database for mySQL.