+++
title = 'project: creating a satisfactory dedicated server in azure'
date = 2024-03-14T13:02:29Z
draft = false
author = "jake armstrong"
ShowReadingTime = true
+++

## purpose

the purpose of this project was to create a vm within azure that was hosting a dedicated server for the game [satisfactory](https://www.satisfactorygame.com/) so that me and my friends could play together online, with the added benefit of it not having to run on one of our computers, and also said host not having to be online for the others to play.

## steps

1. created a new subscription called “GamingServers” with intention to put any servers for games in here, not just satisfactory. i did this via the portal ui, simply going add, and filling in the relevant data

{{< figure src="/images/satisfactory-ds/subscription-creation.jpg" >}}

2.	created a new resource group for satisfactory dedicate server within the GamingServers subscription

{{< figure src="/images/satisfactory-ds/resource-group-creation.jpg" >}}

3.	created a vm within the newly created resource group via the azure portal ui
4.	download private key
5.	ssh via powershell using
    a.	ssh -i (directory of private key) azureuser@ip-of-vm

{{< figure src="/images/satisfactory-ds/ssh-screenshot.jpg" >}}

6.	update ubuntu packages and install steamcmd by running the following:
    a.	sudo apt update ** sudo apt upgrade -y
    b.	sudo add-apt-repository multiverse
    c.	sudo apt install software-properties-common
    d.	sudo dpkg –add-architecture i386
    e.	sudo apt update
    f.	sudo apt install lib32gcc-s1 steamcmd
7.	run steamcmd with “steamcmd”
8.	login anonymously
    a.	login anonymous
9.	download satisfactory dedicated server
    a.	steamcmd +force_install_dir ~/SatisfactoryDedicatedServer +login anonymous +app_update 1690800 -beta public validate +quit
10.	 to start the server you simply need to go to ~/SatisfactoryDedicatedServer and start FactoryServer.sh


## allowing players to turn the server on


it was also a goal of the project for players to be able to turn the server on themselves, without needing me tor anyone else to do it for them. i figured a logic app would be perfect for this, and that a http request could turn it on

1.	create a logic app within the azure portal, give it a unique to azure name, select region, and plan, i selected consumption because it will be used sparingly, not an enterprise amount of times. maybe once or twice a day at most

{{< figure src="/images/satisfactory-ds/create-logic-app.jpg" >}}

2.	open the logic app designer and choose the common trigger of “when a http request is received”. it shouldn’t need a json scheme as it does nothing with the data

3.	add a new step of start virtual machine

{{< figure src="/images/satisfactory-ds/logic-app-design.jpg" >}}

now someone can send an http request to the http post url and it will start the server

## conclusion

overall i am happy with how the project turned on and it worked well for what i needed it for. however there are some parts of it i think could be improved, primarily how players turn the server on. as it is just done via a http request to a known url, anyone who has it can maliciously turn it on over and over again, which could charge me. they can also do it so much that the logic app itself would charge me. 

i did investigate how to work around this issue, but it seemed like it was either awkward, too difficult for myself, or a different method would be required.







