# Setting up Disaster Recovery
We would need to set up DR (Disaster Recovery) to ensure that the applications and data are protected and can be quickly recovered in case of a disaster or unexpected outage.
Full DR<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1c65447e-a834-4fe3-ae8f-6b7b1d2b5609)

After disaster<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1cfe3b94-1399-41e2-923d-b5f9c5ed1988)

Hot vs Cold<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/65ccc554-825b-41d5-b3d5-804322c514f4)

RTO vs RPO
RTO = how long before system is up again
RPO = frequency of data sync to 2nd region (mins)

if RTO = high -> Passive
if RTO = low  -> Active<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/122dd865-5261-4a40-91ff-8676985a92ec)

if RPO = high -> Backup
if RPO = low  -> Sync<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/66867d99-af8f-4c4f-bbc8-d6a5e39ff7c3)

DR of Data<br><br>![![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/143d3aa9-dba8-4fc6-ab78-2264fd8bc902)
download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/90ae6ba4-ddb7-4bf3-ab8f-9e815f619431)
![Pasted image 20230518175442](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9b2b5973-f1ad-402c-bcc8-c34648ce1850)

DR of Compute<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/eeaa95c0-5aff-487a-83d2-c39c3ef303c7)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6796fe16-e669-40ff-a77a-129c9334de0c)



when disaster happens<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/079ea7f9-4f67-4200-a673-733577c06a6b)


DR of Routing<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/20373fd0-5b58-4ab6-8316-ef5e6e8dc481)

![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/865bceee-c366-44b1-b1ba-a9bfcde99b50)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/4625b791-6f6a-4adf-a504-5afdb75b3675)

![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d3bf0a15-716e-43d9-bcd3-65deb42a7253)


implementing DR<br>
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/201dfcd5-e879-41b1-9596-18d0e6655db7)

# 1.Using Azure Traffic Manager
- create new
	- routing method should be 'priority'<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/9e27a585-fe5a-4903-9179-a131d18e4861)

	- change probing interval & tolerated # of failures (every 10 secs & 1 failure)<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/db06567f-b407-4b9f-926e-616c448e27cc)

- implement DR on inventory app service as it uses port 80 which is supported by traffic manager, other ports would be complicated to setup
	- set endpoint<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/32801a71-8026-4b52-9f94-912af09563b7)

	- disable app service authentication, as in order for traffic manager to work with the authentication, we will have to enable custom domain which would make it more complicated. So for the sake of simplicity we disable it<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7b17c1bd-487f-4745-a468-9c677afc0d66)

	- test that it works without authentication<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f0226342-4b4a-4180-9951-369da3d4fb6c)

	- will have to change the free tier to standard, as traffic manager doesnt support free tier<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6224faec-db5c-4b7e-b211-19fd0db0e9c0)

	- lets check the traffic manager
		-  ``http://readit.trafficmanager.net``
		- it means traffic is being routed via traffic manager
		 ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2483dea7-44c1-4f87-b08d-d81eb84490cd)

	- to test DR of inventory app, we will have to create a secondary app to switch over to in case of disaster
		- create new, make sure in the UK West region<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f34dfbd0-1d9b-4a7b-b0a3-05d71ef45617)

		- test the app, it should be a generic app which is what we need for testing<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/efaed417-23a5-4f75-ba7a-e0c50fba1534)

		- add this new app as an endpoint in traffic manager, should have a priority of 2<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/7a9f2231-6b52-42b0-b32a-5c639d16c969)

		 ![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/5a65d0d8-660d-417b-8612-9c654eebc8d4)

	- Time to test
		- check ``http://readit.trafficmanager.net/``
		- it shows primary app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/67a47ec9-b22d-47cd-a63d-17f50b9dd5d2)

		- stop the primary app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/1c9191b9-951d-4e12-b6c5-9c43da534ced)

		- try again
		- it shows secondary app which means traffic manager automatically switched it to secondary<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/12f4c2f4-204b-43ce-9205-d8f37c68f24f)

		- start the primary app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/d5e0856e-5f17-4ef3-bac3-16dc2005bb06)

		- switches back to the primary app<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/73cfa25a-d472-4f2b-a5bf-35885db1d2bb)

# 2.Using Azure Front Door
- create new
	- add frontend
	- add 2 backends
	- set routing rules<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/92a1ebe8-6488-47e6-9db2-556897c6d148)
	![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/b6fb097a-f778-4f2b-b03a-26772da0d169)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/f4cbcdaf-90f5-4f0f-ba6e-e54631b10501)

	
	- test the front door
	- it works<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/e3cffa49-62f3-402e-967e-9c654234271a)

	- stop the primary app and test<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/6fda49bb-63bd-4f91-956e-ff25a3ac0bdf)

	- and it works
	- start the primary app
		- currently the app can be accessed directly, we only want it to be accessed via the front door<br>![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/012dc62b-34e6-4021-ad92-125e80b73a88)
![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/dbb3b23f-b702-4799-8f16-4f640e97dcf7)

		


Final architecture:


![download](https://github.com/salman-cissp/Deploy.WebApp.to.Azure/assets/134168108/2b05fd1a-f332-4d95-8e6b-c3253331b6e7)

