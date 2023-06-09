# Setting up the security
- catalog-vm
	- restrict RDP from local ip only
	- allow https 8080
- weather-vm
	- restrict SSH from local ip only
	- create new subnet `weather-subnet`
	- move vm to new subnet
	- create new nsg `weather-subnet-nsg`
		- attach to `weather-subnet`
		- allow SSH rule<br>
	![Pasted image 20230511155744](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6d59b067-fadf-4d54-80db-a822ac66a649)


- create new vnet `weather-vnet`
	- create subnet : `weather-subnet`
	- move weather-vm to new vnet
		- not possible to move
		- delete & re-create VM
		- keep OS-disk and re-attack to new VM
		- set SSH nsg rule to only allow local public ip
	- vnet-peering
		- between `readit-app-vnet` &  `weather-vnet`
		- weather-vm nsg
			- allow port 8080 only from catalog-vm<br>
		![Pasted image 20230511163658](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9d5c15e9-5985-4823-a329-8f42465ca6ce)

		 
 - Configure Application Gateway
	 - set backend pools
		 - inventory app
		 - catalog app (will have to set later as its in different vnet)<br>
		![Pasted image 20230512125128](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/503ce7df-6ecb-45b8-a227-6d6e4341f267)

	- define routing rules
		- inventory
		- catalog<br>
		![Pasted image 20230512125810](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/57ccb470-940e-4c99-8dee-b612f89a623e)

- Connect Inventory App to App Gateway
	- restrict inventory app access only from the app gateway
		- set 'service endpoint' on `readit-appgw-vnet`
		- set 'access restriction' on app service `readit-inventory-s`
		
- Connect Catalog VM App Gateway
	- peer the 2 networks<br>
	![Pasted image 20230512170627](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/fe9573a4-e7e4-4d5f-aca5-ba6d862f0eb7)

	- add ```catalog-vm``` to app gateway backend pool
	- delete ```catalog-vm``` allow port 8080 nsg rule<br> <br><br><br>
	Architecture so far:<br>
	![Pasted image 20230512194942](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/61e47d49-c873-458e-b97e-1ac63a66c003)
	
