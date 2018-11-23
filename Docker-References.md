#scenario 1:

Docker volumes :
======================

Instructions & Tasks :

For your final task, create a new Docker volume named mission-status on your host machine. Once the volume is created, use the docker cli to display the volume mountpoint on your server. Once you have found the mountpoint, drop to root to move contents found under content-dockerquest-spacebones/volumes/ to local vol directory.

Once this is done, exit root, then start a new container running on port :80 using the base httpd image found in DockerHub, then visit the site in your browser!


###Results :
=======
docker volume create missionstatus
docker volume inspect missionstatus  - copy the mount point
copy your content :      ```cp -r content-dockerquest-spacebones/volumes/* /var/lib/docker/volumes/missionstatus/_data```


#Scenario 2:

Container Logging :
=====================

Instructions & Tasks
Log in to your live environment and sudo to root. Edit the syslog config file and uncomment the two lines under Provides UDP syslog reception. Then, start the syslog service.

Configure Docker to use syslog as the default log driver and then start the Docker service.

Create two new containers using the httpd image. The first one will be called syslog-logging and will use syslog for the log driver. The second will be called json-logging and will use the JSON file for the log driver.

Verify that the syslog-logging container is sending its logs to syslog by running tail on the message log file. Then, verify that the json-logging container is sending its logs to the JSON file. Good luck!


###Answer :

 1. Edit /etc/rsyslog.conf  - > enable UDP settings
 2. Create a file under /etc/docker /daemon.json and add
 {
  "log-driver: "syslog",
   "log-opts": {
       "udp://<privateIP:514"
	 }
 }

 3. reload syslog and docker services
 4. create and run container #docker container run -d --name syslog-logging httpd  
 5. check logs getting updated in /var/log/messages and you can't see logs in docker logs syslog-logging.
 6. create another container with different log driver
	#docker container create -d --name json-logging --log-driver json.file httpd

	now check logs getting updated for json-logging container under #docker logs json-logging not under /var/log/messages


===============================================================================================================================


#WatchTower :
=============
Instructions & Tasks
Welcome to the second learning activity! In this learning activity, you will be using Watchtower to monitor containers for updates. In order to complete this learning activity, you will need a Docker Hub account.

Log in to your live environment and sudo to root.

###Create the Dockerfile
	1.The base image should be node.
	2.Using the RUN instruction, make a directory called /var/node
	3.Use the ADD instruction to add the contents of the code directory into /var/node.
	4.Make /var/node the working directory.
	5.Execute an npm install.
	6.Set ./bin/www as the command.
	7.From the command line, log in to Docker Hub.
	8.Build your image using <USERNAME>/express.
	9.Push the image to Docker Hub.
###Create the demo app

	1.Create a Docker container called demo-app.
	2.The port mapping should be port 80 on the host, mapping to 3000 on the container.
	3.The restart policy should be set to always.
	4.Use the image that you created, <USERNAME>/express.
###Create the Watchtower container

	1.Create a Docker container called watchtower.
	2.The restart policy should be set to always.
	3.Use the -v flag to set /var/run/docker.sock:/var/run/docker.sock.
	4.Use the v2tec/watchtower followed by the -i flag to set the iteration to 30 seconds.
Update the Docker image


	1.Add an instruction to create /var/test. This should be done after creating /var/node.
	2.Rebuild your image.
	3.Push the image to Docker Hub.

	Watchtower will update demo-app

	The Watchtower interval is set to 30 seconds.

	After about 30 seconds, check to see if the container has been updated by executing docker ps.
