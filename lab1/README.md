# Deploy an App on IBM Cloud

Overview
========

In this lab, you will create your first IBM Cloud application by using `Cloud Foundry` service `Liberty for Java™` from the catalog.

Prerequisites
-------------

You will need the following:

-   Internet Explorer, Safari, Firefox, or Chrome web browser
-   An IBM Cloud account [register here](https://console.bluemix.net/registration)


Section 1. Create an application on IBM Cloud
===========================================

In this lab section, you'll create an application using `Cloud Foundry` service `Liberty for Java™` from the catalog.

1.  Log in to IBM Cloud by accessing the url:

    -   https://cloud.ibm.com

2.  Click **Log In** and then enter your login information on the IBM id page and click **Sign in**. You should see your dashboard view:

    ![](images/image9.png)

3.  Click **Catalog**.

    ![](images/image10.png)

4.  Search for catalog entry **Liberty for Java**.

5. Click the **Liberty for Java** tile in the right pane.

    ![](images/image11.png)

6.  Enter a name for your application. The host name information is automatically entered based on the application name. The host name must be unique on IBM Cloud, so enter a name customized with your initials and the date to make the name unique, for example **liberty4java-lz040519**. Keep the remaining defaults including suggested deployment region.

7.  Click **Create**. The application dashboard will open and a status message will appear while the application is starting.

    ![](images/image12.png)

    After a short while, your application will start running and the application dashboard will update with the Running status.

    **Note:** Refresh the browser if the application status did not turn to **Running** after a while.

    ![](images/image13.png)

    Click on the **Visit App URL** link next to the status to open the application in a new tab. This is a simple **Hello World!** application.

Summary
-------

You've now added an application to IBM Cloud. In the following sections, you will learn about the structure and management of Cloud Foundry applications in IBM Cloud.

Section 2. Understand the structure of an IBM Cloud Foundry application
============================================================

In this section, you will learn about structure and deployment of a typical Cloud Foundry application by looking at the app code.

1.  In the previous section, you created an application from **Java for Liberty**. When deploying an application to Cloud Foundry, it combines a specific runtime environment (Java for the app you deployed) with simple code, and optionally one or more services (for example, Cloudant NoSQLDB for the app you deployed) to create the application. Access the github repository which provides the source code for the application at: (https://github.com/IBM-Cloud/get-started-java)

2.  Get a copy of the code using one of these two methods:    

    *   If you have git installed:

        ```
        git clone https://github.com/IBM-Cloud/get-started-java.git
        ```

    *   Use the **Download ZIP** option from the **Clone or download** pulldown on the web page [Git Repository](https://github.com/IBM-Cloud/get-started-java).

3.  Expand the zip file into a new folder, or just change into the folder created by the `git clone` and examine the contents:

    ![](images/image15.png)

4.  IBM Cloud Foundry applications will almost always have a manifest.yml file. This file specifies how to deploy the application and includes the name for the application and optional settings to specify the amount of memory, number of instances, and services that should be bound to the application on deployment. You may open the manifest.yml file in any convenient text editor you prefer.

    ```
    ---
    declared-services:
        sample-nodejs-cloudant-cloudantNoSQLDB:
        label: cloudantNoSQLDB
        plan: Lite
    applications:
      - name: GetStartedJava
        random-route: true
        path: target/GetStartedJava.war
        memory: 256M
        instances: 1
    ```

    This manifest file begins with an extension `declared-services` that is used by the IBM Cloud Continuous Delivery toolchain which is ignored by the Cloud Foundry CLI. We'll ignore this for now. The `applications` section shows typical app metadata. A `name` for the application is always needed. The `random-route` option causes a string to be appended to the application name to form a unique host name. The manifest can specify any needed `services` which are resolved within the Cloud Foundry space for the application. Changing values in the manifest and deploying applications will be covered in a separate lab activity.

4.  When a Cloud Foundry application is deployed to IBM Cloud, the staging environment inspects the files in the application and tries to match the application to a buildpack. A buildpack in Cloud Foundry has a collection of scripts that prepares the application to run on IBM Cloud. The presence of the **pom.xml** file will be used to select a Java buildpack for the application

5.  Examine the **src** folders. These folders contain the source code and related files for this simple application.

Summary
-------

Each IBM Cloud Foundry application combines code, any other assets needed for the application and a dependency file to specify external components or features needed to support the application. When an application is deployed, it is first built into a static object by the buildpack by combining the application assets with a runtime (e.g. Node.js engine) and all project dependencies. After the application is built, it is then copied to containers to run the specified number of instances from the manifest file.

Section 3. Managing an application using the IBM Cloud console
============================================================

1.  In the left-hand navigation for your application dashboard, select the **Overview** item to bring up a summary for the application.

    ![](images/image17.png)

    The **Overview** panel shows the status of the application, the number of active application instances, amount of memory per instance, any connected services and the estimated cost associated with the application.

    The dial on the right side shows how much of your total IBM Cloud Foundry quota is configured to be used by this application. Actual use is less, which is shown on the **Runtime** panel.

    In the upper right of the panel, click on the **Routes** button and choose **Edit Routes**.

    ![](images/image18.png)

    You can add an additional hostname for your application, if you do, wait a moment for the route to be propagated in the Cloud Floudry platform and then select the link added to the **Routes** button. This can also be used to make the application available in a custom domain. IBM Cloud provides full support for external domains and adding your own SSL certificates for external domains.

    In the Overview pane, you can scroll down to see a stream of high-level events for the application in the activity feed and optionally configure continuous delivery to automate builds and deployments for the application.

2.  You can understand more specifics about the application through the **Runtime** panel. Select **Runtime** from the left-hand menu.

    ![](images/image19.png)

    The instances list will show the CPU, memory and disk usage for each instance that is running for the application.

    Notice there is only one instance of the application at the moment. You may easily increase the count of application instances by clicking **+** sign of the **INSTANCES** dial. Accept any warning prompts and the application will be restarted with two instances.

    To make this a permanent change, the manifest.yml file should be updated and the application repushed to IBM Cloud.

    While the application is restarting, select the **Environment variables** tab. From here, you can see the credentials for all services that are bound to the application.

    ![](images/image20.png)

    An IBM Cloud Foundry application will read the `VCAP_SERVICES` environment variable to find credentials for any services that it uses. By getting this configuration from the environment, it's easy to have a single code base work in multiple deployment stages or locations.

3.  Applications in IBM Cloud Foundry write logging and event information to stdout/stderr. You can see application logs and also logs from the IBM Cloud Foundry environment for the applications through the **Logs** panel. Click on **Logs** from the left-hand navigation.

    ![](images/image21.png)

    The **Filter** selector lets you filter out messages from the application as well as parts of the IBM Cloud Foundry environment. Each log entry starts with a code for the source component for the log entry and also the instance number (with 0 for the first instance, 1 for the second and so on).

    Select RTR as the log type:

    ![](images/image22.png)

    These entries show the incoming web requests to the application arriving at the IBM Cloud Foundry perimeter. The host, url, and client information including source IP address of the request are recorded.

    **Note:** If you don't see any RTR log entry, click **Visit App URL** link on the top of the screen. This launches your application and generate RTR log entries. Refreshing the web page of your application a few more times will generate additional RTR log entries.

    Select APP as the log type:

    ![](images/image23.png)

    All stdout/stderr messages from the application are shown. The screenshot example includes some output from the runtime as the application starts and console logs from the application. Your log may have different entries.

    Other common log types you can explore:

    | Type |  Information recorded in log entries |
    |------|--------------------------------------|
    | API  |  Cloud Foundry API calls, such as changes to state like start and stop |
    | STG  |  Messages logged during the application staging (build) process |
    | CELL |  Actions for the container that runs the application |

4.  **Optional as time permits** Click on the **Monitoring** in the left-hand navigation to bring up the application availability monitoring dashboard. By default, each application is configured to be monitored with a simple ping-based availability test.

    ![](images/image26.png)

    Since you've just created this application, there isn't a lot to see here (yet). The examples shown in the lab document is for an application that has been live for about 1 day.

    To see more details, click on the **See Monitoring Details** button.

    ![](images/image27.png)

    You can scroll down through this page to view a summary of any test alerts based upon geography and see a chart of response time by geographic region for your application. You can click on an item in the chart to bring up a card with more information on the item.

    ![](images/image28.png)

    When finished with reviewing the monitoring details, you can click on the arrow next to the app name in the upper left to return to the application dashboard.

    ![](images/image29.png)

5.  When you are ready to delete this application, it is just as easy to remove as it was to add. Follow these steps at any time to remove all traces of the application from your dashboard and IBM Cloud account. From the application dashboard click on the three dots on the right side:

    ![](images/image30.png)

    Select **Delete** from the menu. A prompt will appear to confirm the removal:

    ![](images/image31.png)

    Select any services shown, and then click on the routes tab and select any routes shown, and then click on the **Delete** button.

**Summary**

In this lab, you have seen how to use the IBM Cloud console to deploy and manage an application. Although the application used in this example was very simple, the capabilities and techniques covered apply to all Cloud Foundry applications on IBM Cloud.
