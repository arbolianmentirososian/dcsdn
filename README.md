# dcsdn
This repo stores files used for dcsdn project. It contains of 2 Dockerfiles and scripts for running containers and attaching to existing ones from other terminal.

These files are not enough to run containers. Dockerfiles use COPY command, which copies files/directories from local
file system to container. These files are not uploaded here (too large size), so if you want to have these containeres working, you need to download used in Docker image programs and make sure that they are in proper locations (directories including Dockerfile) and they are named properly (as Dockerfile names them).

To run container you can use run.sh bash script (chmod +x run.sh in containg folder and then ./run.sh command).

If you want attach to running container from other terminal window, use attach.sh bash script (do chmod, as before and after that use ./attach.sh)

Running GUI applications inside Docker container is available after allowing root user access to the X server. 
It can be done by running this command: 
xhost +si:localuser:root

If you want to exit from running container, just type exit in terminal attached to running container.
