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

The python jobs and apache superset are shipped in a container image that can be run anywhere. For the mySQL database, you can run it anywhere. Specifically for Azure deployment, i would recommend Azure Container Instances & Azure Database for mySQL, but you could deploy all of this in a single VM if needed.

### pre-requisites

- Azure Subscription to deploy the Solution (optional if you are to deploy it on-premises)
- Azure Enrollment Billing API Key (<https://docs.microsoft.com/en-us/azure/billing/billing-enterprise-api#enabling-data-access-to-the-api>)
- Get the Azure Enrollment No from <https://ea.azure.com> (requires EA Administrator Login)
- SMTP Server details (for automated email alerts)
- Access to the Azure AD in which these subscriptions are a part of to create a service principal (SPN) to call Graph API's.


- Step 1

Get the Azure EA Billing API Key by following the instructions at <https://docs.microsoft.com/en-us/azure/billing/billing-enterprise-api#enabling-data-access-to-the-api>

Next create a service principal in your Azure AD with atleast read permissions to all the subscriptions for which you want the data.
Follow steps at <https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest>. Copy the details on Application ID, Application Secret, Tenant ID which we will use later while setting the configuration for the Application.

- Step 2

Create a mySQL DB. If you are using Azure Database for mySQL DB follow the steps at <https://docs.microsoft.com/en-us/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal>. You will have to whitelist the Container Instance / Virtual Machine Public IP in the mySQL Server Firewall to allow the Application to access it.

- Step 3

The Application is packaged as a Docker Container. To run the Application you would require an environment that supports Containers. On Azure a quick way to set this up would be to use Azure WebApp for Containers or Azure Container Instances both of which can host an image from a public Docker Registry or a private registry. If you are running this on a Linux Virtual Machine you will need to install Docker Run Time. You can do this by following the instructions at <https://docs.docker.com/install/>.

Running Azure WebApp For Containers : In the step where you select the Container Image select Docker Public Repository & put in "ameetk/azure-billing-alerts-app" as the image name. While this approach would help run the container, for you to configure the container you need to actually SSH into the container. To do so you need to enable the image with SSH. Steps for doing the same are mentioned in <https://docs.microsoft.com/en-us/azure/app-service/containers/tutorial-custom-docker-image#connect-to-web-app-for-containers-using-ssh>

Luckily for you, i have already created an image with SSH server and uploaded to the Docker Hub public repository. The image 'ameetk/azure-billing-usage-app' already has the necessary config to support SSH from Azure Portal.

To SSH into the Container, use the SSH option from the Azure Portal for Azure Web App.
![Connect to Container](https://github.com/ameetkonnur/blogs/raw/master/img/billing-3.gif)


Running Azure Container Instance : In the step where you select the Container Image put in 'ameetk/azure-billing-alerts-app'.
Once done you can connect to the container instance using the 'Connect' feature in the Containers setting for Container Instances.

![Connect to Container](https://github.com/ameetkonnur/blogs/raw/master/img/billing-2.gif)

Running on a Linux VM : In bash type "docker run -it -p 80:8088 ameetk/azure-billing-alerts-app".
Once the container is running you can connect to the container using the below command
"docker run -it exec <container-id> bash" You can get the <container-id> by issuing a "docker ps" command.

- Step 4

Once you are able to SSH / Connect to the container instance, you will have to set a few configurations for the Application to work.

Check the present working directory. It should be /my-app-code/billing-app
If you check the contents of this directory you should see a set of files listed in the image above.

The configuration of the application is in the config.sample.json which needs to be updated and renamed to config.json for the application to work.
Below is a screen shot of the config.json file

![Connect to Container](https://github.com/ameetkonnur/blogs/raw/master/img/billing-4.gif)