![image](img/1920x300_EventBanner_MicroHack_Monitor_wText.jpg)

# MicroHack - Azure Monitor for virtual machines

- [**MicroHack Azure Monitor for virtual machines**](#microhack-azure-monitor-for-virtual-machines)
  - [**MicroHack introduction and context**](#microhack-introduction-and-context)
  - [**Objectives**](#objectives)
  - [**Prerequisites**](#prerequisites)
  - [**Lab environment for this MicroHack**](#lab-environment-for-this-microhack)
    - [Deployment](#deployment)
      - [ARM Deployment](#arm-deployment)
      - [Terraform Method](#terraform-method)
  - [**MicroHack Challenges**](#microhack-challenges)
  - [**Solution Guide**](#solution-guide)
  - [**Finished?**](#finished)
    - [Delete your lab](#delete-your-lab)
  - [**Contributor**](#contributor)

## **MicroHack introduction and context**

Monitoring your infrastructure and applications is important because it provides visibility into the health of backend components that run your applications, allowing you to ensure that critical services are available for users and that they work as expected. Infrastructure monitoring is at the core of the operation, and it boosts reliability by helping you discover current issues and quickly resolving them. It also reduces end-user complaints and keeps you out of trouble.

Application monitoring is also important because it helps you identify issues before they become problems. It allows you to track the performance of your application over time and identify trends that could indicate a problem. By monitoring your application, you can ensure that it is running smoothly and that your users are having a good experience.

Setting up monitoring for your infrastructure and applications is a critical part of any cloud deployment. And it can be a complex task.

This MicroHack scenario walks through the use of **Azure Monitor** with a focus on the best practices and the design principles of how to monitor your overall Azure infrastructure (Iaas and PaaS) and applications accordingly. Specifically, this builds up to include working with an existing infrastructure like Virtual Machines, Application Gateway, Container Apps etc.

**Azure Monitor** is a comprehensive monitoring solution for collecting, analyzing, and responding to telemetry from your cloud and on-premises environments. You can use Azure Monitor to maximize the availability and performance of your applications and services. Azure Monitor collects and aggregates the data from every layer and component of your system into a common data platform. It correlates data across multiple Azure subscriptions and tenants, in addition to hosting data for other services. Because this data is stored together, it can be correlated and analyzed using a common set of tools. The data can then be used for analysis and visualizations to help you understand how your applications are performing and respond automatically to system events.

Use Azure Monitor to monitor these types of resources in Azure, other clouds, or on-premises:

- Applications
- Virtual machines
- Guest operating systems
- Containers including Prometheus metrics
- Databases
- Security events in combination with Azure Sentinel
- Networking events and health in combination with Network Watcher
- Custom sources that use the APIs to get data into Azure Monitor

You can also export monitoring data from Azure Monitor into other systems so you can:

- Integrate with other third-party and open-source monitoring and visualization tools
- Integrate with ticketing and other ITSM systems

![Architecture](./img/azure_monitor.png)

Please be aware that this lab is not a full explanation of Kubernetes (AKS), Containers Apps etc. as a technology. Please consider the following articles required pre-reading to build foundational knowledge.

## **Objectives**

After completing this MicroHack you will:

- Know how to build a monitoring solution for your Azure infrastructure and applications
- Understand default monitoring capabilities of Azure Monitor
- Understand how to use Azure Monitor to monitor your infrastructure and applications

## **Prerequisites**

In order to use the MicroHack time most effectively, the following tasks should be completed prior to starting the session:

- Ensure that you have an active Azure subscription. If you do not have one, you can create a free account [here](https://azure.microsoft.com/en-us/free/).

- Ensure that you have the necessary permissions to create resources in your Azure subscription. If you do not have the necessary permissions, please contact your Azure subscription administrator.

## **Lab environment for this MicroHack**

To experiment with Azure Monitor and learn how to use it, a lab environment is provided. This includes a sample application, virtual machines and other Azure services to generate telemetry data.

Below an architecure diagram displays the setup. Tutorials and documentation that provide step-by-step guidance on how to deploy the enviornment comes along.

![Architecture](./img/architecture_01.png)

### Deployment

There are **two different ways** to deploy the lab environment. The first is using ARM via **Deploy to Azure-Button** and the second is to use the provided **terraform** scripts. The terraform method is the preferred method as it is faster and more reliable. However, if you are not familiar with terraform, you can use the Azure Portal method.

#### ARM Deployment

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fclaestom%2FMicroHack-Monitor-BeLux%2Fmain%2F03-Azure%2F01-03-Infrastructure%2F07_Azure_Monitor%2Fresources%2FARM%2Ftemplate.json)

#### Terraform Method

To deploy the lab environment using **terraform**, click the link below.

- [Deploy to Azure (terraform)](./resources/terraform/README.md)

## **MicroHack Challenges**

- Challenge 1: **[Explore the lab environment, create an Azure Log Analytics Workspace and an empty Dashboard](challenges/challenge-01.md)**
- Challenge 2: **[Configure Virtual Machine Logs](challenges/challenge-02.md)**
- Challenge 3: **[Enable Virtual Machine Insights](challenges/challenge-03.md)**
- Challenge 4: **[Create alerts](challenges/challenge-04.md)**
- Challenge 5: **[Workbooks](challenges/challenge-05.md)**
- Challenge 6: **[Collect custom text logs with Azure Monitor Agent](challenges/challenge-06.md)**

## **Solution Guide**

- Walkthrough Challenge 1: **[Explore the lab environment, create an Azure Log Analytics Workspace and an empty Dashboard](walkthrough/challenge-01/solution-01.md)**
- Walkthrough Challenge 2: **[Configure Virtual Machine Logs](walkthrough/challenge-02/solution-02.md)**
- Walkthrough Challenge 3: **[Enable Virtual Machine Insights](walkthrough/challenge-03/solution-03.md)**
- Walkthrough Challenge 4: **[Create alerts](walkthrough/challenge-04/solution-04.md)**
- Walkthrough Challenge 5: **[Workbooks](walkthrough/challenge-05/solution-05.md)**
- Walkthrough Challenge 6: **[Collect custom text logs with Azure Monitor Agent](walkthrough/challenge-06/solution-06.md)**


## **Contributor**

- [Manuel Kutscher](https://www.linkedin.com/in/manuel-soelch)
- [Judith Freiberger](https://www.linkedin.com/in/judithfreiberger)
