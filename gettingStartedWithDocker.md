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

# The Main Docker Command in regards to Wala

To run the Wala Development image in Docker, you need to run the following command:  
```
docker run -v /mnt/d/WalaTesting/input:/input -v /mnt/d/WalaTesting/output:/output -p 5901:5901 -ti julianwindows/wala:walacodenet
```

# Docker's Run Command

Some basic notes regarding the run command found here https://docs.docker.com/engine/reference/run/:  
```
$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```
There are many different flags you can put in the options sections. I'll only go through a few that were relevant to my experiences.    
Background Mode:  
-d - runs in detached mode like previously stated below. If not specified, deafultly runs in foreground  
Foreground Mode:  
In foreground mode, you can attach the console to a process 's standard input, output,, and standard error.  
-a=[]           : Attach to `STDIN`, `STDOUT` and/or `STDERR`  
-t              : Allocate a pseudo-tty  
--sig-proxy=true: Proxy all received signals to the process (non-TTY mode only)  
-i              : Keep STDIN open even if not attached  

Volume Related Flags:  
-v, -volume=[host-src:]container-dest[:<options>]: Bind mount a volume.  
Mounts folder to container from computer. The 'host-src' is an absolute path or a name value.  
:/input specifies that the path is the input directory  
:/output specified that the path is the output directory

Expose Flags:  
These flags are related to networking.  
-p=[]: Publish a container's port or a range of ports to the host. Both hostPort and containerPort can be specified as a range of ports. When specifying ranges for both, the  number of container ports in the range must match the number of host ports in the range, like in the Wala command:  
       -p 5901:5901  

Other Flags:  
-ti: Opens an interactive container instance  

# Docker Image  
```
$  docker images [OPTIONS] [REPOSITORY[:TAG]]
```
The default "docker images" command shows all top level images, their respective repositories, tags, and their sizes.  
#### Options  
--all, -a: Shows all images  
--digests: Shows digests  
--filter, -f: Filters the output based on provided conditions  
--no-trunc: Prevents the truncation of output  
--quiet, -q: Only shows the image IDs  

Examples taken from docker.com:  
```
$ docker images  

REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE  
<none>                    <none>              77af4d6b9913        19 hours ago        1.089 GB  
committ                   latest              b6fa739cedf5        19 hours ago        1.089 GB  
<none>                    <none>              78a85c484f71        19 hours ago        1.089 GB  

$ docker images java  

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE  
java                8                   308e519aac60        6 days ago          824.5 MB  
java                7                   493d82594c15        3 months ago        656.3 MB  
java                latest              2711b1d6f3aa        5 months ago        603.9 MB  
```
