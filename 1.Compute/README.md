# 1.Pre-requisites
- Install .NET SDK
	- [https://dotnet.microsoft.com/en-us/download/dotnet/6.0](https://dotnet.microsoft.com/en-us/download/dotnet)
	- Make sure to install SDK ver 6, app wont work on latest version
	 
		- check installation<br><br>
		![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4b1bc187-593e-4f40-ba0a-0cb283c0cfb1)

		
- Install VS Code
	- https://code.visualstudio.com/download
- Install extensions
	- C#
	- Azure Account
	- Azure App Service<br><br>
	 ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6a99f639-e40f-41db-8189-0a9836efe2cc)

- Copy Catalog App files<br><br>
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/cb62beb2-0470-4f3e-8e79-c6233614bea9)
	- Load code in VS Code<br><br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9e4d5f87-f0a2-4bd9-a5d4-246e785e5209)
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/01c2f0fd-271c-4aba-b6c0-864b722dc791)

	- F5 to build and run the app 
	- App runs locally<br><br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ee6dbbbb-52a6-4c59-bd2d-4c702e2ecf31)

	
	- The 4 books showing are loaded in-memory. there is no local DB<br><br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e20ae389-c654-46bd-96da-1f139092449c)

	- App is not fully functional, as some of the code is commented out<br><br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/c0d19a85-286f-4641-ab3c-7fc242765037)

	 - Later on, the DB would be connected by changing these values<br><br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/a9e7ddd6-7f27-420a-b8a1-48c712e3d122)
  
# 2.Setting up Catalog .NET App in Azure
This app will de deployed in a VM with IIS

- Prep the code in VS Code : `dotnet publish` 
- Create new VM in azure as Windows Server 2019 Datacenter
- Set the public ip as static
- Install IIS
	- enable Custom Logging
	- enable Logging Tools
- Download .NET 6 'Hosting Bundle'
- Copy the code in VM
- Setup new website in IIS
	- check if its working locally in the VM
- Test website from local PC using public ip, it wont be accessible (will fix it later)

# 3.Setting up NodeJS Weather API in Azure
This app will be installed as Linux VM

- Create new VM in azure as Ubuntu Server LTS
- Set the public ip as static
- Connect to VM via putty
- Install weather app
	- install git
	- copy code from github : `sudo git clone
	- install node packages : `sudo install npm
- Start the app : ``npm start
	- app listening on port 8080
- Connect catalog VM with weather VM
	- insert weather VM private ip in the website
- Test website from local PC using public ip, it wont be accessible (will fix it later)<br>
![Pasted image 20230508120735](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8552b9cc-65e2-43de-bf32-1591fa557533)

# 4.Setting up Inventory App Service
This app will install as a web app 

- VS Code
	- add 'inventory' app to the project<br>
	![Pasted image 20230519114015](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/49fed479-7c5b-42a1-b618-0e8549f69816)
	- compile and test by running locally
- Azure
	- create App Service Plan (Free Tier)
	- create web app
	- upload the published code from vs code to web app
	- deploy directly from VS Code :'Deploy to web App'<br>
	![Pasted image 20230508151717](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b2e0c5f7-025d-46f6-b739-4a8f2eec5403)


# 5.Setting up Shopping Cart in Azure
This app will be installed as a docker container and managed by Kubernetes
- Install
	- docker desktop on local PC
	- docker extension in VS Code
	- 'azure cli' on local PC
- add 'cart' app to the project
- compile and test by running locally
- containerize 'cart' app 
	- create and build an image using a DockerFile & the app code<br>
	 ![Pasted image 20230508191741](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1da0bc76-78f5-4cf8-820d-1eb210d26e5d)
	 ![Pasted image 20230508191310](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1138b24b-cfb5-4e87-b7a6-5d0f80434ceb)
	
	- run the container to test its working locally
	- create new azure container registry from VS Code
	- upload the container to azure container registry<br>
	 ![Pasted image 20230508195050](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/68b1ba28-3f2a-47ab-aee7-5bae2cf33fe0)

- Create AKS cluster
	- attach it to ACR
	- pull image from ACR
	- deploy container into AKS<br>
	![Pasted image 20230508214341](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/eaf8069d-9f1b-4870-ba50-24ac44d6f60e)
	![Pasted image 20230508214353](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/05654652-7e2b-4a36-b64c-a3126d84d31a)
	![Pasted image 20230508201822](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9f8db44f-ca8f-41bb-9eb0-6bccfcccc5fd)

	- AKS creates a service, which is an end-point from were we can access the container<br>
	![Pasted image 20230508213423](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/95356e99-17c8-4b74-bf35-069eec128f27)

# 6. Setting up Order app
This app doesn't have a front-end. It works in the back-end to process the orders sent to the shopping cart. It will be deployed as a serverless app (function). Its functionality cant be tested directly from a web browser like the other apps , so we will have to use a tool 'Postman API'

- add 'order' app to the project
- install 'postman api platform' (to test http trigger) app on local pc 
- compile and test by running locally<br>
	![Pasted image 20230510163701](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/a65f9d13-32fc-4491-b2dd-52bc4cefeefe)
	
- create Function App in azure
	- deploy directly from VS Code :'Deploy to Function App'<br>
	![Pasted image 20230510163916](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9f2e93b4-e696-44c4-baf8-1bdfa3aa7793)

	- check it on azure<br>
	![Pasted image 20230510163942](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/3a0c73ce-53c1-4262-ab2f-1b6cfb425add)

	- test the function<br> 
	![Pasted image 20230510170457](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/db5a019b-f7ae-4149-b593-c0956d5af8c9)
	![Pasted image 20230510170940](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/3ae86ee2-f69f-4a94-ba3f-fecd2b3f91d3)
	![Pasted image 20230510171100](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8d0902f0-a569-4752-8012-b452bb369d4e)
  
# 7. How to choose Compute Type
![Pasted image 20230510171249](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/45ac28ad-68cb-4871-8555-f3f42b88f0e0)

