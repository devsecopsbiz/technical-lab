[![Build Status](https://dev.azure.com/MSREADY19Sandbox/devsecopsbiz-session/_apis/build/status/technical-lab?branchName=master)](https://dev.azure.com/MSREADY19Sandbox/devsecopsbiz-session/_build/latest?definitionId=4&branchName=master)

# Microsoft Ready, Feb 2019
## AI-APP-ST300: Hands on Dev*Ops (Dev-Sec-Ops-Biz) 

**Welcome to Technical Lab for the AI-APP-ST300: Hands on Dev*Ops session!**

DevOps has been propagating throughout the world as the place to be in order to be Agile and successful. 
As maturity sets in, new challenges start to come up, namely the security role and the integration with the business teams. 
As such, buzz words like _DevSecOps_ and even _DevSecOpsBiz_ are becoming more popular in discussions. 

This lab session builds on top of _AI-APP-ST201: The new trends of Dev*Ops (DEV-SEC-OPS-BIZ)_, allowing you to experience first hand the construction of a real world scenario.

## Prerequisites 
Before we get started we need to ensure some prerequisites required for the lab.

* Azure Account [Azure Portal](https://portal.azure.com)
* Azure DevOps Services Organization [Azure DevOps](https://dev.azure.com/)

## Lab steps
* Start by [creating an Azure DevOps Project](#AzDevOps)
* Now let's [setup the build and release for the application](#CICD)
* Next, we'll want to [include Security into the pipeline](#Security)
* And to wrap up, let's [gather Business information](#Business)


> Optional: Connect Azure DevOps to Power BI by [creating an active bugs report in Power BI based on a custom Analytics view](#PBI)

*********

<a name="AzDevOps"></a>
# Lab: Create Azure DevOps Project 

This workshop will guide you through the initial setup of an Azure DevOps Project, providing a quicker, prebuilt setup ([option 1](#Option#1:-Azure-DevOps-Labs-++)), and a step by step setup ([option 2](#Option-#2:-Azure-DevOps-from-scratch)).

## Setup Azure DevOps Project

>  Use Azure DevOps Labs to create a preconfigured project and enrich it for a quick start.

* Start by creating a new Project using a preselected [Lab](https://azuredevopsdemogenerator.azurewebsites.net/?name=WhiteSource-Bolt&templateid=77362)
    1. Navigate to the Azure DevOps Demo Generator and *Sign In* with the credentials you've used to create the Azure DevOps Organization:
    
        <img src="img/AzureDevOpsLab-SignIn.png" width="800" />

    2. Select your Organization from the dropdown, fill in the Project Name, and *Create Project*:

        <img src="img/AzureDevOpsLab-CreateProject-01.png" width="800" />
    
    3. Wait for a few seconds for the project to create and then *Navigate* to it:

        <img src="img/AzureDevOpsLab-CreateProject-02.png" width="400" />

* Install additional extensions from the Marketplace:

    1. Navigate to `https://marketplace.visualstudio.com/items?itemName=keesschollaart.arm-outputs`, selecting the *Get it free* button.

        <img src="img/ARMOutputs-navigate.png" width="800" />

    2. Now, select the recently created Azure DevOps Organization on the dropdown, and press *Install*.

        <img src="img/ARMOutputs-install.png" width="300" />

    3. After a few seconds you should see a success message. Now, select the *Proceed to organization*.

        <img src="img/ARMOutputs-success.png" width="300" /> 

* Create a free account for *WhiteSource Bolt* 

    1. Navigate to *WhiteSource Bolt* under *Pipelines -> WhiteSource Bolt*.

    2. Fill in your details to create a free account.

        <img src="img/WhiteSourceBolt-freeaccount.png" width="400" />

    3. After a few seconds, we're good to go!

        <img src="img/WhiteSourceBolt-success.png" width="400" />

* Import additional Git repos, required for the labs:

    1. Navigate to *Repos* and on the upper repositories drop down, select *Import repository*:

        <img src="img/AzureDevOpsLab-ImportRepo.png" width="800" />

    2. Fill in the *Clone URL* with `https://github.com/Microsoft/SmartHotel360-Website.git`
    and press *Import*:

        <img src="img/AzureDevOpsLab-ImportRepo-CloneURL.png" width="400">

    3. Select *Import repository* again, now filling with     `https://github.com/Deliveron/owasp-zap-vsts-extension.git`
    and press *Import*.


## Next step:  
[Setup the build and release for the application](#CICD)

*********

<a name="CICD"></a>
# Lab: Azure DevOps CI/CD

This lab will guide you through building Continuous Integration (CI) and Continuous Deployment (CD) pipelines with Azure DevOps. The build pipeline will make use of a Java application, built with Maven, but also a .NET Core and Azure Functions App.
For the release pipeline we'll be leveraging ARM templates and Azure App Services to host our application.

## Create Build Pipeline

* Wait a few seconds for the import process to finish and navigate to *Pipelines*, *Builds*:

    <img src="img/AzureDevOpsLab-Builds-01.png" width="400">

    ### WhiteSource Bolt Build
    1. Under *New*, select *Import a pipeline* to import a precooked pipeline to build the recently imported repo:

        <img src="img/AzureDevOpsLab-Builds-02.png" width="800">
        
    2. Drag and drop the `SmartHotel_Petchecker-Web.json` file, located on the Lab repository under *~/files/SmartHotel/*, or Browse for the same file. Then, press *Import* to start the import process:

        <img src="img/AzureDevOpsLab-Builds-03.png" width="400">
    
    3. Select the `Hosted VS2017` Agent pool:

        <img src="img/AzureDevOpsLab-Builds-04.png" width="800">
  
    4. Now select the Source Repo to the recently imported repository:

        <img src="img/AzureDevOpsLab-Builds-05.png" width="800">

    5. To finish, enable the *Continuous integration* trigger, saving the build after it:

        <img src="img/AzureDevOpsLab-Builds-trigger.png" width="800">

    ### OWASP Build
    
    Now we'll be using an alternative way to setup our CI build, leveraging a YAML file.

    6. Navigate to the `owasp-zap-vsts` repo, and drag and drop the `azure-pipelines.yml` file, located on the Lab contents you've downloaded, under *~/files/OWASP*


## Create Release Pipeline

* Under *Pipelines*, navigate to *Releases* and press `New pipeline`

    <img src="img/AzureDevOpsLab-Releases-01.png" width="800">

    1. We first need to create a dummy, empty, Release pipeline in order for the *Import* to become available. 
    Let's do this by pressing `New pipeline`, `Empty job` and `Save`:

        <img src="img/AzureDevOpsLab-Releases-02.png" width="800">

    2. Navigate back to `All pipelines` and `Import a release pipeline`:

        <img src="img/AzureDevOpsLab-Releases-04.png" width="800">

        <img src="img/AzureDevOpsLab-Releases-05.png" width="400">

    3. Drag and drop, or navigate, to select the `SmartHotel360_Website-Deploy.json` file:

        <img src="img/AzureDevOpsLab-Releases-06.png" width="400">

    4. Press *OK* to start the import process. You should now have a release pipeline like this:

        <img src="img/AzureDevOpsLab-Releases-07.png" width="800">

    5. Navigate to the Stage tasks:

        <img src="img/AzureDevOpsLab-Releases-08.png" width="800">

    6. Select the first task and, under *Azure Subscription*, press `New`:

        <img src="img/AzureDevOpsLab-Releases-09.png" width="800">

    7. This opens a pop up where we'll have to fill in the details of our *Azure Subscription*, where we'll be deploying out solution:

    > Step in each one of the tasks with error, selecting and authorizing an Azure Subscription to use in the lab. 

    9. Remove the existing *Artifact*.

        <img src="img/RemoveArtifact.png" width="800">

    10. Now, add the `SmartHotel_Petchecker-Web` build artifact. This will make sure we're using the artifact comming from the build we've created before.

        <img src="img/AddArtifact.png" width="800">

    11. To finish, enable the Continuous Deployment trigger

        <img src="img/AzureDevOpsLab-Releases-trigger.png" width="800">


#### Run a test build

1. In Azure DevOps, click on Builds, select each build and click the "Queue" button on the right upper corner.

    <img src="img/AzureDevOpsLab-Builds-Run-01.png" width="800">

2. Monitor the builds and wait for the build to complete

   <img src="img/AzureDevOpsLab-Builds-Run-end.png" width="800">

3. The release will automatically start when the build is complete (be patient, this can take some time). Review the results as it is complete. 

   <img src="img/AzureDevOpsLab-Releases-Run-executing.png" width="800">

4. Navigate to the deploy logs and check the details regarding the last step, `Deploy SmartHotel Website`.

   <img src="img/AzureDevOpsLab-Releases-CheckOutput-deploytask.png" width="800">

5. Go to the end of the log, and grab the URL of the destination website.
It should look something like this: `https://smarthotel360lcu4bmxi7kl4w.scm.azurewebsites.net/api/deployments/21548112613183`

    Grab that value.

    <img src="img/AzureDevOpsLab-Releases-CheckOutput-1.png" width="800">

6. Using this, remove the `scm` and `api` details, ending up with a URL like: `https://smarthotel360lcu4bmxi7kl4w.azurewebsites.net/`

    Navigate to the new URL and you should be see the website we've just deployed.

    <img src="img/AzureDevOpsLab-Releases-CheckOutput-site.png" width="800">


7. Now kick-off the full CI/CD pipeline by making an edit to the  code in the Azure DevOps code repo.
Navigate back to *Repos*, *SmartHotel360-Website* repo, select `appsettings.Development.json` and press *Edit*

    <img src="img/AzureDevOpsLab-Repo-edit.png" width="800">

8. Change the *Name* property value to something you want, and *Commit* the change after it

    ![Repo change to trigger CI CD](img/AzureDevOpsLab-Repo-commit.png)


## Next step:  
[Include Security into the pipeline](#Security)

*********

<a name="Security"></a>
# Lab: Security

This lab will guide you through adding some Security validations into our pipelines, detecting and fixing security vulnerabilities, but also problematic open source licenses.

## Review WhiteSource Bolt task configurations on Build

*WhiteSource Bolt for Azure DevOps* is a FREE extension, which scans all your projects and detects open source components, their license and known vulnerabilities.

It covers over 200 programming languages support and continuous tracking of multiple open source vulnerabilities databases like the NVD, security advisories, peer-reviewed vulnerability databases, and popular open source projects issue trackers.

### Let's start adding it to our pipelines

1. In Azure DevOps, click on "Pipelines" on the left menu and then click "Builds"

2. Select each build pipeline, click the "Edit" button, and select the `WhiteSource Bolt` task

    <img src="img/AzureDevOpsLab-Builds-WhiteSourceBolt.png" width="800">

3. Now navigate to *Pipelines*, *WhiteSource Bolt* on the left and explore the generated reports

    <img src="img/AzureDevOpsLab-Builds-WhiteSourceBolt-report.png" width="800">


## Add OWASP ZAP penetration testing tool to the Release pipeline

OWASP is a worldwide not-for-profit organization dedicated to helping improve the quality of software. ZAP is a free penetration testing tool for beginners to professionals. ZAP includes an API and a weekly docker container image that can be integrated into your deployment process

### Let's start adding it to our pipelines

1. Start by setting up a Docker host for later use:

    ## Deploy a template with the Azure Docker VM extension
    Let's use an existing quickstart template to create an Ubuntu VM that uses the Azure Docker VM extension to install and configure the Docker host. You can view the template here: [Simple deployment of an Ubuntu VM with Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

    You need to login onto the [Azure Portal](https://portal.azure.com), and access the Cloud Shell, setting it up to *Bash*.

    <img src="img/AzureCloudShell.png" width="800">

    After a few seconds you should see something like this:

    <img src="img/AzureCloudShell-ready.png" width="400">

    Cloud Shell already provides us the latest Azure CLI, allowing us to execute the following commands.

    First, create a resource group named *msReadyDevOps* in the *eastus* location:

    ```azurecli
    az group create --name msReadyDevOps --location eastus
    ```

    Next, deploy a VM with that includes the Azure Docker VM extension from [this Azure Resource Manager template on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). When prompted, provide your own unique values for *newStorageAccountName*, *adminUsername*, *adminPassword*, and *dnsNameForPublicIP*:

    ```azurecli
    az group deployment create --resource-group msReadyDevOps \
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
    ```

    It takes a few minutes for the deployment to finish.


    ## Deploy your first NGINX container
    To view details of your VM, including the DNS name, use [az vm show](/cli/azure/vm):

    ```azurecli
    az vm show \
        --resource-group msReadyDevOps \
        --name myDockerVM \
        --show-details \
        --query [fqdns] \
        --output tsv
    ```

    SSH to your new Docker host. Provide your own username and DNS name from the preceding steps:

    ```bash
    ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
    ```

    Once logged in to the Docker host, let's run an NGINX container:

    ```bash
    sudo docker run -d -p 80:80 nginx
    ```

    The output is similar to the following example as the NGINX image is downloaded and a container started:

    ```bash
    Unable to find image 'nginx:latest' locally
    latest: Pulling from library/nginx
    efd26ecc9548: Pull complete
    a3ed95caeb02: Pull complete
    a48df1751a97: Pull complete
    8ddc2d7beb91: Pull complete
    Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
    Status: Downloaded newer image for nginx:latest
    b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
    ```

    Check the status of the containers running on your Docker host as follows:

    ```bash
    sudo docker ps
    ```

    The output is similar to the following example, showing that the NGINX container is running and TCP ports 80 and 443 and being forwarded:

    ```bash
    CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
    b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
    ```

    To see your container in action, open up a web browser and enter the DNS name of your Docker host:

    <img src="img/nginx.png" width="400">


2. Before proceeding any further, let's run a OWASP ZED Attack Proxy against our published website.

    > Note: Replace the website URI with the one you've deployed earlier  

    ```bash
    docker run -v $(pwd):/zap/wrk/:rw -t owasp/zap2docker-weekly zap-baseline.py \
        -t https://smarthotel360lcu4bmxi7kl4w.azurewebsites.net -g gen.conf -r testreport.html
    ```
    Docker image will be pulled and the container will be then executed.

    <img src="img/OWASP-docker-pull.png" width="400">

    When if finishes you should see a summary like this

    <img src="img/OWASP-finish.png" width="400">

    Now take a quick look at the report html

    ```bash
    cat testreport.html
    ```
    
3. Now let's grab a personal access token (PAT) we'll be needing later on.
    > Save that PAT token, it can't be retrieved later

    2.1 Navigate to the user profile and select *Security*

    <img src="img/PAT-token-security.png" width="200">

    2.2 Select *Personal access token*

    <img src="img/PAT-token-navigate.png" width="800">

    2.3 Create a new personal access token

    <img src="img/PAT-token-create.png" width="400">

    2.4 Copy the token, making sure you don't lose it.

    <img src="img/PAT-token.png" width="400">

3. Now let's add some `Command Line` tasks to our release pipeline, one for attaching the generated analysis report and another to be able to create bugs if necessary:

    <img src="img/Add-OWASP-tasks-navigate.png" width="800">
    <img src="img/Add-OWASP-tasks-search.png" width="800">

4. Paste this command on the *Script* text box, changing the values between `<>`, as seen in the image.

    `$(System.DefaultWorkingDirectory)/owasp-zap-vsts CI/drop/owasp-zap-vsts-tool/bin/Release/owasp-zap-vsts-tool.exe attachreport collectionUri="<Azure DevOps ORGANIZATION URI>" teamProjectName="MsReadyLab" releaseUri=$(Release.ReleaseUri) releaseEnvironmentUri=$(Release.EnvironmentUri) filepath=$(System.DefaultWorkingDirectory)\OwaspZapReport.html personalAccessToken=<GENERATED PERSONAL ACCESS TOKEN>`

    <img src="img/Add-OWASP-tasks-Report.png" width="800">

5. Paste this for the *Create bugs* task, changing again the values between `<>`.

    `$(System.DefaultWorkingDirectory)/owasp-zap-vsts CI/drop/owasp-zap-vsts-tool/bin/Release/owasp-zap-vsts-tool.exe createbugfrompentest collectionUri="<Azure DevOps ORGANIZATION URI>" teamProjectName="MsReadyLab" team=Demo releaseUri=$(Release.ReleaseUri) releaseEnvironmentUri=$(Release.EnvironmentUri) filepath=$(Agent.ReleaseDirectory)\OwaspZapAlerts.xml personalAccessToken=<GENERATED PERSONAL ACCESS TOKEN>`

    <img src="img/Add-OWASP-tasks.png" width="800">


## Next step:  
[Gather Business information](#Business)

*********

<a name="Business"></a>
# Lab: Azure DevOps Business

This workshop will guide you through the process of creating and configuring a set of specialized dashboards, each with a diferent scope that can be used by distinct team members and stakeholders. 

## Create Build Pipeline

1. Create [Queries, Dashboards, charts, & widgets](https://docs.microsoft.com/en-us/azure/devops/report/dashboards/overview?view=vsts)


*********

<a name="PBI"></a>
# Lab: Connect Azure DevOps to Power BI

In this lab, we'll show you how to create a custom Analytics view for active bugs and then generate a Power BI report based on that view. You'll learn how to filter the Analytics view, create a trend chart, as well as a card showing the current count of active bugs. 

 Using Power BI connected to Analytics views, you can create reports on your work items. Reports can show your current work data and also historical revisions of the work items.  

Follow these steps to create a report in Power BI desktop that shows a **daily active trend of  bugs** based on a custom **Analytics view**:

1. From the web portal for Azure DevOps:   
	a. Open **Analytics views**  
	b. Create a custom Analytics view for your data.   
2. From the Power BI Desktop:  
	a. Get the dataset defined by the Analytics view   
	b. Create a trend report  
	c. Apply filters to your trend report  
	d. Create a card and apply the Is Current=True filter to show current count.  

<a id="prerequisites">  </a>
## Prerequisites  

In order to create a Power BI report that references an Analytics view, you must meet the following criteria:  

- Have installed *Power BI Desktop* *February 2018 Update* or later version. You can download this client application from the official [Power BI Desktop download page](https://powerbi.microsoft.com/desktop).
- Have tracked bugs for some period of time on which to generate a trend report. 


## Create a view to only include Active bugs

<img src="img/open-analytics-views-vert.png" width="400">

From the web portal, **Analytics view**, choose **New View** to create a custom view. Fill out the forms provided in each tab as shown in the steps below. 



Choose **Continue** to move to the next tab in the panel. If you need to revisit a tab, simply click the tab title. 

### 1. Name your view and set to Private  

1. Give your view a name, such as Active bugs. Select **Private view** to save it under **My Views**. Otherwise, you can select **Shared view** to save it under **Shared Views**.  

    <img src="img/active-bugs-general.png" width="400">

Choose **Continue** to move to the next tab. 

2. From the **Work items** tab, select the Project and Team that contain the data you want to report on.  
 
    Here we choose the FabrikamFiber project and the Fiber Suite App team.
    
    <img src="img/active-bugs-team.png" width="400">

	To add additional projects or teams, click *Add row icon* to add a new row and then select the project and team. 
    

3. Under **Backlogs and work items**, add a new row, and then select **Bug** from the list of work items types.

    <img src="img/wi-type-picker-bugs.png" width="400">

4. Under **Field criteria**, choose the **State** field set to **Active**.

    <img src="img/active-bugs-field-criteria.png" width="400">
    
	Choose **Continue** to move to the next tab. 

5. In the next tab, **Fields**, we'll keep the most common fields selection. This selection will also include any custom fields that have been added to your project.  
    
    <img src="img/fields-common.png" width="400">

	Choose **Continue** to move to the next tab. 

6. In the **History** tab, select **Rolling period in days** and enter **60** days. Keep the **Granularity** set to **Daily**. These selections will generate 60 snapshot rows of data for each active bug. Each day will include all the details of that work item as it was at the end of that day. 
    
    <img src="img/active-bugs-history.png" width="400">

	Choose **Continue** to move to the next tab. 

7. On the last tab, **Verification**, click **Verify view**. The system will verify your view by running a test query against the dataset it defines and validate all your filter criteria. 

    <img src="img/active-bugs-verify.png" width="400">
  
	> Verification time will vary based on the amount of data defined in your view. Verify your view to make sure all the definitions are correct.  

	Once your view successfully verifies, choose **Save**. You can then start using it in Power BI. Verification also returns an estimate of the number of rows in the dataset and the time it will take to load in Power BI.

    <img src="img/active-bugs-verify-success.png" width="400">
	
	If your view fails to verify successfully, you'll get an error explaining the issue and pointing to a possible fix. Try changing the options you selected in the **Work Items** and **History** tabs to include less data, and then verify the view again. 

## Open Power BI desktop and load your view 

1. Open Power BI Desktop.  
 
2. Choose (1) **Get Data**, choose (2) **Online Services**, then (3) **Azure DevOps (Beta)**, and then click (4) **Connect**. 

    <img src="img/get-data-2.png" width="400">
    
3. Enter your organization name, the same Azure DevOps name you used to create the Active Bugs view, and the same project for which you defined the Active Bugs view. 

    <img src="img/specify-account.png" width="400"> 

4. Expand the **Private Views** folder, choose the **Active Bugs** view you saved in the previous section, and then click **Load**. 

    <img src="img/choose-view.png" width="400">

## Create a daily bug trend report 

1. In your report, (1) select the Line chart visual, (2) enter `work item id` in the search field, and then (3) check the box for **Work Item Id**. 

    <img src="img/select-line-chart-work-id.png" width="400">

	This will change your chart to a single dot.  
  
2.	Click the context menu icon next to the **Work Item Id** field and change it from **Count** to **Count (Distinct)**.

    <img src="img/choose-count-distinct.png" width="400">

3. Next, (1) select the **Date** field in the fields list as your axis. To see a daily trend, (2) click the context menu icon next to the field and change it from **Date Hierarchy** to **Date**.

    <img src="img/select-date.png" width="400">

   To view trends over time, you want to use **Date** and not **Date Hierarchy**. The **Date Hierarchy** in Power BI rolls ups everything into a simple number for the period. The day level in the hierarchy rolls all days to a number between 1-31. For example, April 3 and May 3 both roll up into number 3. This is not the same as counting items per actual date.

## Filter your trend by Priority

1. To group your bugs by `Priority`, search for the field in the list (a). Drag the Priority field into the `Legend` of your trend chart (b)
    
    <img src="img/filter-priority-legend.png" width="400">

2. The chart now shows a daily distinct count of bugs, grouped by Priority.

3. To just show only Priority 0 and 1 bugs, click the context menu icon next to the **Priority** field to set the filters.  

    <img src="img/filter-priority-1-and-2.png" width="400">   
   
	The trend chart is now grouped and filtered by high priority bugs.

## Create a card for latest bug count

1. Add a new page by clicking the plus sign (+) at the bottom of the page. 

2. Select the card visual, add the **Work Item Id** field, add the **Work Item Type** field, and click the context menu icon next to the field and change it from **Count** to **Count (Distinct)**.

	The card now shows the number of active bugs times the number of days they existed within the selected time period.  For this example, that number is 1066. 

    <img src="img/card-active-bugs.png" width="400">   
    
3. To get the latest count, add **Is Current** as a page level filter, and select the True checkbox. The **Is Current** field is added automatically to all datasets, marking the rows that contain the latest revision of the work items.

    <img src="img/iscurrent.png" width="400">   
   
	The value on this card should match the count on the last day of the trend chart, which you can obtain by returning to Page 1 and hovering over the last day as shown.  

    <img src="img/check-of-is-current.png" width="400">

## Save your report, optionally share with others 

1. Choose **File > Save As** to save your report to your local workspace. 

2. To publish your report to Power BI, choose the **Publish** tab.

    <img src="img/publish-powerbi.png" width="400">

## Continue to explore your data

Analytics views provide you with a great deal of power and flexibility to filter your data and generate useful reports quickly and easily using Power BI. Within a custom view, you can create datasets that span multiple teams or projects. 

*************

## AI-APP-ST300: Hands on Dev*Ops (Dev-Sec-Ops-Biz) 

**END**