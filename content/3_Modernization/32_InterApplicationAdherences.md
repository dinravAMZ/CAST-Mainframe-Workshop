---
title: "Inter-app adherences"
chapter: true
weight: 2
---

# Drill-down to adherences between applications 

**Objective**: Adherences between Mainframe applications are usually a blocker for the migration.  In this exercise, you will identify the complexity behind the dependency between 2 applications in order to determine the best strategy to make the migration smoother.  

**Documentation**: This exercise will utilize the CAST Imaging [App to App Dependencies](https://doc.castsoftware.com/display/IMAGING/User+Guide+-+Application+to+Application+dependencies+scope) feature. 

## Adherences between applications 

For the moment, you sill drill-down to ***CardDemo*** and ***Express*** applications to confirm which wave they should be part of.  

Click on the link between the applications which is a Cobol to Cobol remote call from ***Express*** to ***CardDemo***.  

![Cobol Remote Call](/images/Sce2CobolLink.png) 

![Cobol Remote Call](/images/Sce2CobolLink2.png)
Select the **caller** object ***COACTUPC*** from Application ***Express*** and right-click then **Add > Callers**. You can see that there are 3 programs in ***Express*** that are directly calling ***COACTUPC***. 

![Source Code Details](/images/Sce2SrcCode.png) 

Select the object ***COACTUPC*** and click on the right-hand menu **Object** section then **Properties**. Here you can see all characteristics of the object including the Cyclomatic Complexity and the transactions in which the object is involved.  

> :bulb: **Tip:** You can see in the **Properties** section that the cyclomatic complexity is very high > 600, this means that COACTUPC is a very complex program inside application ***CARDDEMO***, we can expect this program to hold heavy business rules.  

Scroll to the bottom of the **Properties** section to select the transactions associated to the object. 

***COACTUPC*** is involved in only one transaction named ***CAUP***. 

![Transaction Details](/images/Sce2Trans.png)

Select the ***CAUP*** transaction in the drrop-down list, then  you will be redirected to the ***CARDDEMO*** Application Drill Down.  

![Transaction View](/images/Sce2CAUP.png)

You can navigate the Transaction View to understand the components involved with **COACTUPC**. For instance, you see that a CICS Transaction actually exists to access CAUP transaction. You can view the source code of this CICS Transaction on the right menu. 

![Transaction View](/images/Sce2CAUPcode.png)

## Determine a strategy 

We learned in a few clicks that the application ***Express*** has several calls to ***COACTUPC*** program, which is only used by one transaction in ***CARDDEMO*** application. We can think of different strategies to either: 

1. Extract the full ***CAUP*** Transaction move it into ***Express*** application: this would be possible if ***CAUP*** transaction is only reading data and transferring back to ***Express***. In that case, there would be no functional constraint to keep ***CAUP*** Transaction inside ***CARDDEMO***. 

2. Expose the ***COACTUPC*** program with an API. For this, we need to study the type of dependencies involved with the Cobol Program to be sure to secure all calls to sensitive components and data sources.

### Decoupling strategy 

You can rapidly check the feasibility of the first strategy by looking at database accesses in ***CAUP*** transaction.  

First reset the Zoom to see the full transactions. 

![Transaction Zoom Reset](/images/Sce2zoomReset.png)

Click on the right-hand menu on **Tags** then on **Output**. All end points of the transaction will be selected, meaning all objects being the last called in the call graph starting with ***CAUP***, in this case VSAM Files, CICS Datasets and one Cobol Program. 

![Transaction Outputs](/images/Sce2outputs.png)

You can now zoom in towards the database objects and on the right-hand menu, you will have access to **Relationships**, where you can see which links are READ and which ones are READ.WRITE. 

You see 2 links from the paragraph ***600-WRITE-PROCESSING*** towards CICS Datasets ***CUSTDAT*** and ***ACCDAT***. 

![Transaction DB Access](/images/Sce2WriteAccess.png)

The decoupling strategy is thus not a good idea here because ***CAUP*** transaction also writes data, it would have too much impact to extract it out of ***CARDDEMO*** application. 

#### API strategy 

The next strategy that can be considered is to expose the ***CAUP*** transaction so that ***Express*** application can access it. 

Getting back to the list of outputs of ***CAUP*** transaction, you did see the VSAM files, but we also have  external program ***CEEDAYS***. 

Click on the edge leading to ***CEEDAYS*** and display the code.  

![Transaction External program](/images/Sce2Ceedays.png)

**You now have identified 2 actions to before you can expose** ***CAUP*** **transaction with an API:**

* Secure the READ.WRITE accesses to the VSAM files 

* Secure the access to the external program ***CEEDAYS*** 

Now you can annotate the view with the actions you have identified so that the development team will understand the important issues to tackle before exposing the transaction through an API. 

![Transaction Annotation](/images/Sce2annotation.png)

**With a rapid investigation of a dependency between two applications, down to the source code and the related transaction and data sources, you have been able to outline a bewt candidate strategy to handle the dependency between the applications and go forward in the modernization initiative.** 