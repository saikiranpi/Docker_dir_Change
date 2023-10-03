# Docker_dir_Change
Changing Docker /var/lib/docker(Root Directory )  to another location.

Step 1
To ensure a smooth process, it's essential to halt Docker before making any changes. You can use the following systemd command to stop Docker
$ sudo systemctl stop docker.service
$ sudo systemctl stop docker.socket

Step 2
Now, let's proceed to modify the Docker configuration by editing the /lib/systemd/system/docker.service file. This file is responsible for managing Docker within systemd, and we'll specify the new location inside this file. You can use the text editor of your choice, such as nano, to open it.
$ sudo nano /lib/systemd/system/docker.service

Step 3
The line requiring modification appears as follows:
ExecStart=/usr/bin/dockerd -H fd://

Edit the line by adding -g followed by the desired new location for your Docker directory. Once you've made this change, remember to save the file and exit the text editor.
ExecStart=/usr/bin/dockerd -g /new/path/docker -H fd://

Step 4
If you haven't done so already, create a new directory at the location where you intend to relocate your Docker files.
$ sudo mkdir -p /new/path/docker

Step 5 
Next, you can transfer the contents from /var/lib/docker to the new directory. A recommended approach for this task is to use the following rsync command:
$ sudo rsync -aqxP /var/lib/docker/ /new/path/docker

Step 6 
Afterwards, refresh the systemd configuration for Docker to reflect the changes made earlier. Once that's done, you can initiate Docker.
$ sudo systemctl daemon-reload
$ sudo systemctl start docker

Step 7
To verify that the changes were successful, execute the "ps" command to confirm that the Docker service is indeed using the new directory location.
$ ps aux | grep -i docker | grep -v grep
After entering the above command it looks like this.
![image](https://github.com/saikiranpi/Docker_dir_Change/assets/109568252/7935a0bf-b4c4-437c-bfab-4e0b6bbe88c9)
Please refrain from any confusion. I've created a separate volume named "/10gbdata," so the highlighted portion should resemble this. 

For your reference, please refer to the image below: [![image](https://github.com/saikiranpi/Docker_dir_Change/assets/109568252/4782ff2f-802c-4853-af15-b8aa93ae47e2)
 ]


