---
title: "Portfolio Analysis "
chapter: true
weight: 1
---

# Determine the best modernization strategy at portfolio level 

**Objective**: In this exercise, you will identify the quick wins in a portfolio of applications, check the adherences between applications, identify the necessary actions on the cluster and estimate the feasibility of the different scenarios.  

**Documentation**: This exercise will utilize the CAST Imaging [App to App Dependencies](https://doc.castsoftware.com/display/IMAGING/User+Guide+-+Application+to+Application+dependencies+scope) feature. 

## Portfolio View 

Connect to Imaging URL and select ***App to App Dependencies***. 

If you are already connected to an application, you can switch to ***App to App Dependencies*** from the Applications drop-down menu. 

![App to App Dependencies](/images/Sce1App2App.png) 

![App to App Dependencies](/images/Sce1App2App2.png) 

The next screen allows you to select specific applications to visualize or search among applications in your portfolio. 

For this exercise, just leave the screen as-is with all applications selected and click on ***Done***. 

![App to App Dependencies Welcome Page](/images/Sce1Welcome.png) 

You are now able to visualize applications as bubbles and interactions between them represented by edges. 

> :memo: **Note:** CAST recognizes interactions such as Shared Data (common tables between applications), Local/Remote procedure calls (Call to program), File Access, Data Access, MQ protocol (IBM MQ Publisher), MQE Queue call, AMQP, MQTT, HTTP and Websockets (RabbitMQ Exchange), HTTP/REST, HTTP/SOAP, etc. 

The applications have not yet been selected for any wave, so you need to identify the technical constraints to choose the best wave. 

At first sight, the application ***Recipe*** is isolated from the rest of the applications, which makes it a good candidate for a Wave 1. 

Now focus on the 2 applications ***Express*** and  ***Carddemo***. Those applications have adherences but are isolated from the rest of the system. They seem good candidates for a Wave 2. 

> :memo: **Note:** CAST Imaging automatically tags the applications with relevant technical details extracted from the CAST analysis: level of program coupling, technology stacks, level of data coupling. Custom tags can also be manually created or injected into CAST Imaging through a rest-API such as the department name, recommended wave, etc. 

Select ***Recipe*** application and visualize the properties on the right hand menu. Now look for the button **Add Tag** on the left-hand menu and add a Tag "Recommended Wave 1". 

![Adherences between applications](/images/Sce1AddTag.png) 

![Adherences between applications](/images/Sce1TagAdded.png) 

Select the application **CARDDEMO** and check the right menu ***Properties*** where you have technical information on the application size. 

We can see that this application is quite small with no technology blockers and only one link with application ***Express*** which is bigger but still has a reasonable size. 

![CardDemo application](/images/Sce1Carddemo.png) 

Those 2 application would be candidates for a Wave 2 as they seem to be tightly coupled, they need to be migrated together. But we need to analyze the dependency between them to refine this assumption. This will be covered in the next exercise. 

For the other applications in the system, many adherences have been found. The type of adherence can be filtered on the right-hand menu in **Relationship** section.  

![App2App Relationships](/images/Sce1Relations.png) 

You can identify the most complex adherences from their type, see below reference: 


| Protocol - Type of link | Complexity |
|----------|-------------|
| HTTP/REST - Web to Spring MVC |  Low |
| Redirect - CICS to CICS  | Medium |
| Data Access - JCL to JCL | High |
| Shared Data | Very High |

Additionally, a number of technical **Tags** have been automatically created to help identify the main blockers: 
* Data Coupling 
* Program Coupling 
* Technology Blockers 

Tags can be used to filter applications using the search option. 

![App2App Search by tag](/images/Sce1TagSearch.png) 

Use the **Tags** to select the applications part of Wave 4 based on the criteria below: 

![App2App Waves definition](/images/Sce1WaveDef.png) 

![App2App Wave 4](/images/Sce1Wave4.png) 

For your reference, the Data and program coupling tags have been created in Imaging based on the following rationale which can be adapted for each application portfolio context: 

| Number of links  | Complexity |
|----------|-------------|
| <5  | Very Low |
| Between 5 and 20 | Low |
| Between 20 and 35 | Medium |
| Between 35 and 50 | High |
| >50 | Very High |


All the links between applications can also be exported in in **Excel** using the Export button on the bottom left-hand menu. 

![Adherences between applications](/images/Sce1Export.png) 

![Adherences between applications](/images/Sce1ExportXls.png)  

**You have now been able to retrieve consequent technical insights on the application portfolio to help in building a first draft or Wave recommendation through CAST Imaging.** 

You will now drill down in the adherences between 2 applications in the next exercise to refine your assumptions. 