<h1>Deploy Web App to Azure</h1>

 
<h2>Description</h2>
In this project we take a .NET web app, test it locally first, then deploy it to Azure. We then improve it by adding networking, security, database support, AD authentication, messaging and disaster recovery.<br>
The main goal is to leverage Azure's cloud platform to enhance the app's scalability and performance.
<br>By deploying to Azure, we can take advantage of features like automatic load balancing and seamless integration with other Microsoft services.
The project will be using the following services:<br><br>

- <b>Virtual Machines</b>
- <b>Web App Service</b>
- <b>Kubernetes</b>
- <b>Function App</b>
- <b>Application Gateway Load Balancer</b>
- <b>Azure SQL</b>
- <b>Cosmos DB</b>
- <b>Redis</b>
- <b>Event Gris</b>
- <b>Azure AD Authentication</b>
- <b>Key Vault</b>
- <b>Traffic Manager</b>
- <b>Front Door</b>


<h2>Languages and Utilities Used</h2>

- <b>.NET Core 6</b> 
- <b>NodeJS</b>
- <b>VS Code</b>
- <b>Postman API</b>

<br>Project starts with this:<br>
![Pasted image 20230508120735](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/fcef9900-f827-45ac-97f6-5549ce3bd90d)



<br>All the way to this:<br>
![000044](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/c5c8fab2-bf16-429f-9997-54d5d7680456)






<h2>About the Web App </h2>
<h4>Contains 4 main services:</h4>

• Books Catalog<br>
• Inventory Management<br>
• Shopping Cart<br>
• Order Engine (runs in the background) <br>

There is an additional Weather service just to showcase implementation of API <br>
Check the video to get how the app would work once it gets deployed<br>

https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/500e705b-8bed-4b8a-93d4-209c98e69322

<h2>App Architecture:</h2><br>

![Intro](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/49621139-8fee-4131-b48c-66ad291d4b1e)

<h2>Project Walk-through:</h2>

## [1.Setting up the Compute](1.Compute/README.md)<br><br>
We will set up the compute resources including Virtual Machines, Web App Service, Kubernetes, and Function App.
## [2.Setting up the Networking](2.Networks/README.md)<br><br>
We will set up the networking infrastructure including Application Gateway Load Balancer, Traffic Manager, and Front Door. This will ensure efficient traffic management and high availability for the web app.
## [3.Setting up the Databases](3.Databases/README.md)<br><br>
We will set up the databases required to support the web app, including Azure SQL, Cosmos DB, and Redis. This will enable efficient data management and scalability for the web app.
## [4.Setting up the Disaster Recovery](4.DR/README.md)<br><br>
We will set up disaster recovery measures to ensure business continuity in case of any unforeseen events. This will include setting up Traffic Manager and Front Door for one web app.
## [5.Setting up the Policies](5.Policies/README.md)<br><br>
We will set up policies to ensure that resources and services are used in a secure, compliant, and cost-effective manner. We will set up policies for resource tagging and resource usage restrictions
## [6.Managing the Costs](6.Costs/README.md)<br><br>
We will see how to monitor and manage the costs associated with all the resources used in the project.
## [7.Getting Insights](7.Insights/README.md)<br><br>
We will get full observabilty by taking an inventory of all the resources and keep a track of them to identify any potential issues or opportunities for optimization.



