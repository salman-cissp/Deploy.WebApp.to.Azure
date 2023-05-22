# 0.Which version to choose
![Pasted image 20230513112421](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/09da912f-3fd7-492c-a6ff-b248f7fbf72f)

# 1.Setting up the SQL Server
- create new sql db ``readitdbserver-s
- enable public access 
- Connect to catalog app
	- add the db connection string in appsettings.json<br>
	 ![Pasted image 20230513140436](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/11096b25-767c-4652-9136-53d64b7ba10f)

	- disable in-memory db<br>
	 ![Pasted image 20230513140540](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/fd50e0bd-d9a7-487b-b834-174575c1919c)

	- create table<br>
	 ![Pasted image 20230513140917](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7363b9c4-ca48-4663-afac-af57d4a776b3)

	 - create this table in azure database<br>
	 ![Pasted image 20230513141158](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9160903c-003d-4c7d-875f-2d3bc805ce82)
	 - Books table created<br>
	 ![Pasted image 20230513141436](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8c8c8520-96c0-4744-9071-12a9adfd7a6b)

	 - build code locally, add books to db<br>
	 ![Pasted image 20230513141719](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/40023bc8-787b-48fd-892f-36ade839516c)


	 - check if records were added<br>
	 ![Pasted image 20230513141817](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f3f08679-50a8-486c-a2af-acfc3881a653)
	 ![Pasted image 20230513141817](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/53a54bd3-81a0-40c1-bc70-03d8139b3b00)

	 - this means catalog app is working and successfully connecting to azure db
	 - publish it again
		 - stop web server
		 - copy the contents of publish folder to web server
		 - start web server
	 - connect to catalog via the app gateway public on port 8080
	 - wont be able to connect<br>
	 ![Pasted image 20230513201220](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/52cbf2b6-7298-4e11-86b7-6369c4b57b25)

	 - add public ip of catalog-vm pip to the firewall rule in sql database server<br>
  	 ![Pasted image 20230513201843](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2c6aac1b-ca68-43c6-b460-2cf4179303d6)

	 - should work now<br>	 
	 ![Pasted image 20230513202533](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4b15da43-6257-4d3f-8b66-c6a24231c910)

	 - another another layer of security by setting service end point between the db and catalog vm
		 - ``readit-app-vnet`` to ``default`` subnet of ``catalog-vm``<br>
		 ![Pasted image 20230513202955](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/22b631bc-156e-43e0-92df-8ffcf2f10219)
		 
		 - in db, add existing vnet in firewall<br>
		 ![Pasted image 20230513203206](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/97e5d26d-52e3-4526-a040-b76de4684e0a)
		 ![Pasted image 20230513203226](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e93aca77-ac63-417e-97e8-b446e376efbc)

		 - previous catalog-vm rule can now be removed<br>
		 ![Pasted image 20230513203343](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b98993be-2381-4714-9300-cbf378ec0658)

		 - make sure catalog-vm can still connect to the db(using service-endpoints)<br>
	  	 ![Pasted image 20230513203508](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d391b151-fc7a-400d-8c2c-a412b5f01b98)

	 
- Connecting Inventory to DB
	- inventory code in VS Code
		- uncomment<br>
	![Pasted image 20230514110559](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/322d048d-a45d-4d2c-9174-5edd558aed07)

		- deploy and publish to web<br>
	![Pasted image 20230514110825](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/a7ab81b2-0a55-4689-8db1-d9f597dd580c)
![Pasted image 20230514110918](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f73ebc4e-a6cf-456b-8441-cafdbb3d6c4d)

	
		- check inventory app @ ``https://readit-inventory-s.azurewebsites.net/<br>
	![Pasted image 20230514111234](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/defc833c-5ec2-4d3c-9512-c1655b44c607)

		- this errror is because of this:<br>
	![Pasted image 20230514111355](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e5625555-f143-410a-92ce-56fbf8c458d7)

		- we need to add the connection string to the web app, but not in the code<br>
	![Pasted image 20230514111631](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/252144d6-4109-48fc-8916-151a442bd917)

		- now we get this error<br>
	![Pasted image 20230514111748](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/54973323-cf0b-4ca5-9dbf-49cb0a8f97e6)

		- add the ip address to db firewall<br>
	![Pasted image 20230514111906](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/c377b12f-a0c1-4925-be99-e53c78206e80)

		- test it works<br>
![Pasted image 20230514111958](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1876c247-4d16-4fb0-b071-699612f6d5e8)

		- change stock of one book<br>
![Pasted image 20230514112341](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/aff34f0a-1527-4dab-b5f1-035fa13bc4a0)

		- test if catalog updates<br>
![Pasted image 20230514112416](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d19bcd61-cc2b-4da4-8412-8cb488ab53df)

		- means both inventory & catalog are connected to the same azure sql db  and working fine<br>
    
# 2.Setting up the Cosmos DB
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/16e43c24-d3c5-4955-aecf-076b2175c913)

- create new cosmos db
	- ``readit-cosmos-db-s
	- create new database & container
		- ``readit-orders
		- ``orders<br>
		![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ca14c1d4-e8b3-4b34-a1dc-81d1b32c0ac6)

	- add sample items to ``orders`` using data explorer<br>
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/112fe127-3fca-49c8-8bce-685021dcf76f)

	- test by running sql command<br> ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2b030c8d-d8c3-4be7-98a3-66b191a861fc)

	- load 'order' code in VS Code
		- uncomment<br> ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/edd81253-ac7e-4c9b-b624-59cf3078af8b)

		- copy connection string from cosmos db to the code<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e4086639-9cfe-4f7a-8e9c-711ef3ba489e)

		- build it
	- check the 2 functions  against sample order using postman api<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/89f487b3-dc8d-4d13-9393-7d8d51c503c2)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/86886a26-8968-452d-b39d-208f94d8d995)

	- works fine<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7f14b968-10a4-416c-9512-2602276a15bb)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/78b2c84b-5430-4a5e-bae4-81dc44d15095)

	 
	- check in cosmos db 'data explorer', new item gets created<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/77b592d4-0f35-467c-a3c3-3ecbcd2c55fd)

	- since its working, its time to deploy the code to azure
	- Deploy 'order' function to Azure<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8358226e-181b-4d59-9364-2d0ff5aa3e52)

		
	- upload settings<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ff5d2fa0-43b7-4837-ab31-97604136ddf3)

	- check functions on azure<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/66daed24-4167-4645-9e9c-1ffebd702f40)

	- get function Url<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/66806e31-4ba2-4440-9578-3f6846fb4c59)

	- test it in postman api<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/46e328fe-a375-4439-8f01-31017cc76f98)

	- check that it created entry in the table<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/70d06875-332d-44b2-8160-5aa8b32d7e82)


	- check if this came from the function app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8062693f-8a54-41c4-b5da-1cd23f4666f1)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8ea53959-d892-4b00-9100-0330f4e16733)
	
	- this proves azure function connected to cosmos db, triggered by http trigger
  
# 3.Setting up the Storage Account
- create new account
	- ``` ordersreadit0523 ```
	- create new container
		- ```neworders```
		
# 4.Setting up the Redis
Redis is a tool that helps make web applications faster by storing frequently used data in memory. It can also help different parts of an application talk to each other quickly. Think of it like a really fast and efficient way to store and retrieve information for your app.

Setup for the shopping cart. When items are added to the cart from catalog, it will be stored in the redis

- create
	- ``readitredis0523

Connect to Catalog:
- VS Code
	- catalog
		- add connection string in 'appsettings.json'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/c32fe1ba-d52f-4c31-8171-7b7c9e40dc28)

		- uncomment code in 'index.cshtml.cs'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f3b66dfc-4bc3-459a-80cd-59934010461e)

	- run locally
	- test
		- cart shows 3 items which were added previously<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/dd86e812-f0aa-499a-bd40-7b43b26b05af)

		- remove one book<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/718c161f-afe2-402d-a4b9-385780bfec1f)

		- catalog should now show 2 also<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/389942ee-d8b0-4439-b5c2-f8037714f4a6)

		
		
 - Publish to azure
	- web server on ``catalog-vm
		- stop server
		- copy published files from VS code to catalog folder 
		- start server
	- test
		- if it shows 2 books in the cart, it means it is connected to redis
		- add another book<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e4f441da-e61e-4c3d-a73c-7db558247eef)

Connect to Cart:
- VS Code
	- cart
		- add connection string for redis & DB in 'appsettings.json'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5c642d43-d654-42e0-881e-0247e8ae6d52)

		- get 'Function Url' from ' ``ProcessOrderCosmos`` from function app and add to ``OrderFunctionUrl`<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/98bc1fb8-17fc-4427-a453-7ecc537503f2)

		- uncomment code in 'index.cshtml.cs'<br>
	 ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8b5988b2-f054-4a54-83a1-2c98538f7f98)

 - Placing Order
	 - go to function ``ProcessOrderCosmos
		 - check logs while placing order from the cart 'Code+Text'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/85532040-5bc3-4e27-850b-4d848295846f)

		 - place order from the local code in VS Code<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4e25a94b-a846-4d2d-830c-ed30a29e161d)


		 - order is placed as the logs show<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b0cfd910-461a-497a-ac0d-c5658ba833f9)
		 - check Cosmos db which show the order<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ef636a3f-a190-480b-a003-92a1dbfc2a97)

		 - so far we are running the cart code locally, it needs to be from azure as a container
			 - build image<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/bff137f6-c4a4-4b5b-a740-b0f82809d4a8)

			 - deploy to ACR<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/161283a8-cf4e-4b55-b11d-636a2f670ee9)

			 - deploy to AKS
				 - ``kubectl apply -f deployment.yam
				 - test<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/39cd9528-57f1-4eef-ad11-2d47c8cdd915)
![Pasted image 20230515104915](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/bc75c79c-d79e-4a2f-a0c4-e795d6018e66)

				 
				 - add 2 books from catalog<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/de2532eb-95c9-472f-99dd-ccea7f824d41)

				 - now check the cart we just deployed in AKS<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/cddff928-28a4-428f-8200-125d6a72adb8)

				 	 
				 - that means docker image in AKS is connected to redis & azure sql db
				 - is it also connected to azure function which will place the order?
					 - place the order<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/cf172ba9-03ae-4528-8c25-63924a21d12e)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/bb6b85e5-8ac6-41be-9d44-45a9e5dbcba2)

					 
					 
					 - check the logs in ``ProcessOrderCosmos`` function<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5b8d4293-c3ca-4d0c-8668-268574daa06e)

				- so we now have the catalog, inventory , cart and the order function all working together
				- The most important part is that we successfully connected redis to catalog and cart app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/cce54716-1752-477d-9d46-d73e61357632)

				- Are we done ?<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/592291f5-fe56-4bd7-beb2-222f918a3a42)

# 5.Setting up Messaging
A Messaging service in Azure helps different parts of an application talk to each other, even when they're not all online at the same time. It's like a mailman that delivers messages between different parts of an application. This can be useful in many different applications, like online stores, financial apps, and Internet of Things (IoT) solutions.

![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/baf3b50a-dbb9-4057-a29d-38372a8d1ea6)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6ce2d7e7-fda4-47a3-a23e-0e53521649e4)


Instead of activating function though http trigger(public & synchronous), we use Event Grid(much safer & asynchronous) 
- Event Grid |System Topics
	- create
		- ``orders-topic``<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2df33c04-8260-4507-be96-d876f29ea521)

		- in ``order`` app replace ``ProcessOrderCosmos.cs`` with new one which supports event grid
		- add event grid support<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e2774939-222f-4818-bd7b-545d38015c08)

		- get connection string from ``ordersreadit0523`` storage account<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/80ec3982-a8e4-44d4-b5ff-45808e8c9415)

		- paste it in ``local.settings.json``<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b075bfc3-9b1d-4250-8ce6-34fc69d7cb71)

		- copy connection string from ``readitfuncstorage0523`` storage account and paste into new entry in ``local.settings.json`` <br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9c76e6d8-eafe-417c-8c28-c41c40ef21ef)

		- deploy this function to ``readitfunctionapp0523``<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9d17f005-a470-4be6-b2e8-f3dc1b98d020)

		- upload settings<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7c4cd58f-3193-4775-a8c8-b7777893d519)

		- create event subscription & select our function as subscription to this topic
			- whenever event in storage account occurs, our function is going to trigger<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/67ff4eb9-7c9f-43f4-a6a2-2de6de486811)

		- test
			- upload blob in storage account<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2be6859b-7337-4044-878e-89c89dd7254f)

			 
		- connect cart app to storage account so that when placing order the order will be saved in the storage account
			- replace ``index.cshtml.cs`` with new one in the cart app
			- we dont need OrderFuntionUrl anymore<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/59bf7360-94bd-41f1-a852-8601b0784901)

			- replace it with storage account connection string<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8c12a9e0-17cd-4a83-a125-804044d2b2ee)

			- build<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2c130557-a017-4e21-8213-b5138aea86eb)

			- test locally
			- open catalog, add 2 books<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b43e566e-f0a8-45fe-94f2-9448e156fdc6)

			- check cart<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/50220b47-1ddd-44bd-9fb3-d6897b87539e)

			- place order<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7064677a-22e1-456e-8115-9192b0eea164)

			- order is received<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/37c98ad3-2a46-4ea0-ac42-1dee87f6d117)

			- function received the order<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/29756d09-6423-4d6f-96d4-0ef0b0f9199e)


			- order received in cosmos db<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/39f1fbca-1e87-4a45-8bf3-ad81f6d53386)
		- publish cart app to AKS
			- build docker image in VS Code
			- push image to ACR as ver 1
			- update deployment.yaml with ver 1
			- deploy to AKS
			- test
				- add one book<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/950477aa-5354-47fc-b3cc-25cf3885985d)

				- check the cart<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7dbfd2dd-25f5-48e0-bca2-5a0af8db6dd1)

				- place order<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d889b344-609b-40d8-87db-3c104971bf26)

				- check neworders<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/55b27420-0d11-48ad-aaa8-5f7cc317084c)

				- check function<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ff3ac111-baf7-48a6-a000-13294d72b93f)

				- check cosmos db<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8dc3d8a5-368e-4bbd-80b1-4594d5422765)

			- everything works fine
- Protecting the function (publicly available)
	- allow only from event grid<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/59d41422-459e-4d7b-a9b4-ed9e563884fd)

 


![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/3ca1f14e-28e8-44a7-9fe3-a042848d30b4)


# 6.Setup Active Directory Authentication
The inventory app is only for authorised employees to manage the books inventory, we will use AD to secure it

Azure AD Licences<br>![Pasted image 20230516110305](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/3070fa29-60ad-4ef8-b79d-72f3509768bf)


We are going to assign managed identity to inventory app and use it to connect to azure sql db
- turn on system managed identity for ``readit-inventory-s`` to register with azure ad <br>![Pasted image 20230516112913](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/02f4b7a0-343d-4917-8e0d-acd233e58df5)

-  configure ad admin for sql server<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/a568947f-4d13-4219-834a-cccd4526e8de)

- set a user as admin<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/868099fd-7bb7-4eaa-8d7c-31b2be7d9bff)

- run sql commands against ``readit-db
	-  create a new user ang grant it read, write and admin permissions<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d947cae1-bc37-444a-82f3-79f516ad77f3)

	- in ``readit-inventory-s`` remove credentials from connection string as we have set managed identity<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/15869368-3b40-45b7-9d85-dc40bec1ba2a)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e51ba454-9136-4a87-8bb1-e7d859935cfe)

	 
	- update inventory app with AD support
	- build
	- deploy
	- test<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/426d9e09-f4fc-41f3-a02d-81c31bb328b4)

	- its working
	- configure AD authentication
	- register app in AD
	- add code to use azure ad as auth engine
	- auth uses OAuth & JWT<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/43dfa022-b030-40ae-8097-e07cb45e9a7e)


	- remove access restrictions from ``readit-inventory-s``<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ed8a2d51-1f1a-498e-aabf-4e9e323f98b0)
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6dd1269d-89dc-48b2-b0a4-0edaf9667f86)

	- register this app in azure ad<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/245a8dff-1d26-400a-af77-8dea94c2dfd1)

	- configure token<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5731d5d0-f1f2-41cf-8848-8433d4cd9464)

	- add client secrets<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5760b9f1-62b9-45a5-984e-0b5e83a5600c)

	- now integrate this auth in our app service
		- add identity provider<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4e06a229-2b48-49c4-8472-8f433b39d09f)

		- make changes to the code<br>
		- publish![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/351043dc-75da-4770-b403-b6289e022cea)

		- test
			- when trying to access the website it asks for permission<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/87fe8ace-d830-4988-91c6-fb4f9c030077)

			- it successfully authenticates user<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d396d088-c8fe-4b27-9c21-bfbb15c26661)

			- also test with other user<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e56f83f2-0fc4-45ed-9d86-86a82c3fe493)

			- this is what the token contains<br>
			![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5c98c903-6160-4659-824b-41a34a5671d9)

			![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/722046bf-fa97-4cdc-91e8-2eb9ffdc781f)
			![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7d9e6473-1578-45b4-9261-591fae5e00c6)


- Azure AD B2C
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/fa97d111-6a51-46c7-b923-3ab3dbfada5f)

	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/328c50e6-3b21-40d6-9273-44ae1b82b6ef)

	- Quite complex to setup, so wont be using it for this app, but is an option
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/10de5674-3b49-4ade-8083-4885ea280cf5)

# 7.Setting up Monitoring
Monitoring apps in Azure is crucial to catch issues early and prevent downtime or data loss. Azure provides monitoring tools like Azure Monitor, Application Insights, and Log Analytics to help collect and analyze data from your apps, infrastructure, and network. By setting up monitoring, you can detect issues early, troubleshoot problems, plan resources better, improve security, and provide a better user experience.
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1cb062f7-07e0-4a89-bd9b-ec8c941dcd76)

- create new Analytics Workspace which would collect all the logs in one central place
- enabling monitoring for inventory web app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/247cc8bd-3c02-44b3-b819-1eac83ba6e78)

- check the realtime longs in 'Log Stream'
- set more logs in 'diagnostic settings'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ca3842a0-36b7-460d-854d-3c6a3c56ab20)

- check the logs in log analystics workspace selecting the right scope<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d84cb048-0bce-448d-9f99-761231f5cec3)

- check insights which are configured automatically<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/8e70ddc7-548e-4730-a5a7-47cd4017d02d)<br><br>

![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/70c343c4-8250-4949-a404-1720211217e1)

# 8.Setting up Security
Having connection strings in the code is very risky, to make it more secure we are going to store them in azure KeyVault
- KeyVault
	- create new<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1aa7a9c3-739a-421d-9063-7325813038ee)

	- connect via private endpoint<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/05abfa26-6107-498d-91ab-e4a05335a357)

	- Change catalog app code to support keyvault
		![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ed380d7a-10fd-43a2-94c9-d7a1a3f0d383)

		- add diagnostic setting so we can see who accessed our secrets<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5f53d960-d1df-4ff5-b917-9b62137218bd)

		- add firewall rule to allow access from local ip<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e13dff78-adc3-41fc-a4e1-dc8ec3980fcd)

		- define secret to define connection string
			![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4c70cc5d-bb73-4787-9f2d-a9d7ab4ce080)

			- copy the values from the code. Name is very important, it should match the values in the code<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/04c6b9c9-5bdc-49b3-8965-cb59cba9cf5b)
			- copy vault uri<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b61602b3-f4b1-4e4b-98bd-5e5e01df5c87)

			- remove the connection string from the code and add the keyvault<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/bd5ea56e-7797-430c-9bf1-a4e9a2412ddc)


			- build
			- test locally, should see this which means it can connect to the db via the keyvault and not the connection string<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2ce6b96e-31f0-45dc-b074-bdcb67ce96b7)

			- deploy the code to catalog-vm IIS
			- you should get this error, because keyvault has no idea who we are<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/cd7208a9-a271-43b8-842b-3307636c5c10)

				- add managed identity in catalog-vm<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/316cd563-977e-4961-bb2f-829c3f055c3c)

				- give catalog-vm access to secret in keyvault<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/ad402154-51f4-4dee-a82b-a483c99180f1)

				- test locally<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5fb9ac4d-0c21-48db-99dd-b30efe4061bc)

				- now check via public ip<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/dd5f1e14-55a5-496f-a769-ecf96fcf4a71)

				- check the access history<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/a1e75441-afc6-4097-8ad7-059c8d4e79b6)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/864dbef7-0003-4a18-abd8-6ae467930f50)

				
- Security Center
	- check the recommendations<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/20e9d7d6-6492-4548-8133-453e0c212a6b)

	- check the alerts<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/daa08475-e1e3-469d-8434-1892af70628e)

	- check all the inventory being monitor<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5df2eb0f-83ba-4307-805c-421c27a2b299)

<br><br>


![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/3d3ebfce-5e69-4181-87c1-296be67137a6)

		 
		 
  
