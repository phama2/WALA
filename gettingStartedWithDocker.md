Documentation of my progress on learning Docker and its various commands. I learned about docker mainly through its getting started page here: https://docs.docker.com/get-started/.

This initial Part follows the tutorial:
Through docker, open a terminal. Since I installed docker through WSL, I run the command 
```
sudo service docker start
```
Then run the following commands:
```
docker run -d -p 80:80 docker/getting-started
```
The flags being used here have different meanings:  
-d - run the container in detached mode (in the background)  
-p 80:80 - map port 80 of the host to port 80 in the container  
docker/getting-started - the image to use  

This starts the basic docker tutorial. 

Some basic notes regarding the run command found here https://docs.docker.com/engine/reference/run/:  
```
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```
There are many different flags you can put in the options sections.  
Background Mode:  
-d - runs in detached mode like previously stated below. If not specified, deafultly runs in foreground  
Foreground Mode:  
In foreground mode, you can attach the console to a process 's standard input, output,, and standard error.  
-a=[]           : Attach to `STDIN`, `STDOUT` and/or `STDERR`  
-t              : Allocate a pseudo-tty  
--sig-proxy=true: Proxy all received signals to the process (non-TTY mode only)  
-i              : Keep STDIN open even if not attached  

# Docker in regards to Wala

To run the Wala Development image in Docker, you need to run the following command:  
```
docker run -v /mnt/d/WalaTesting/input:/input -v /mnt/d/WalaTesting/output:/output -p 5901:5901 -ti julianwindows/wala:walacodenet
```

The flags here mean the following:

