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
```
-d - runs in detached mode like previously stated below. If not specified, deafultly runs in foreground  
Foreground Mode:  
In foreground mode, you can attach the console to a process 's standard input, output,, and standard error.  
-a=[]           : Attach to `STDIN`, `STDOUT` and/or `STDERR`  
-t              : Allocate a pseudo-tty  
--sig-proxy=true: Proxy all received signals to the process (non-TTY mode only)  
-i              : Keep STDIN open even if not attached  
```

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

# Docker ps
```
$ docker ps [OPTIONS]
```
This incredibly useful command lists all the current containers on your machine.
#### Options  
```
--all-tags, -a : Show all containers (default shows only running)
--filter, -f : Filters output based on provided conditions
--last, -n : Show n last created containers, including all states (default is -1)
--latest, -l : Shows the latest created container, including all states
--no-trunc : Prevents truncating of the output
--quiet, -q : Only displays container IDs
--size, -s : Displays total file sizes
```
(All Examples taken from docker.com)  
Examples:  
Prevent truncating output:   
```
$ docker ps

CONTAINER ID        IMAGE                        COMMAND                CREATED              STATUS              PORTS               NAMES
4c01db0b339c        ubuntu:12.04                 bash                   17 seconds ago       Up 16 seconds       3300-3310/tcp       webapp
d7886598dbe2        crosbymichael/redis:latest   /redis-server --dir    33 minutes ago       Up 33 minutes       6379/tcp            redis,webapp/db
```

Show disk usage by container  
```
$ docker ps -s

CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS       PORTS   NAMES        SIZE                                                                                      SIZE
e90b8831a4b8   nginx          "/bin/bash -c 'mkdir "   11 weeks ago   Up 4 hours           my_nginx     35.58 kB (virtual 109.2 MB)
00c6131c5e30   telegraf:1.5   "/entrypoint.sh"         11 weeks ago   Up 11 weeks          my_telegraf  0 B (virtual 209.5 MB)
```
       
# Docker Image  
```
$  docker images [OPTIONS] [REPOSITORY[:TAG]]
```
The default "docker images" command shows all top level images, their respective repositories, tags, and their sizes.  
#### Options
```
--all, -a: Shows all images  
--digests: Shows digests  
--filter, -f: Filters the output based on provided conditions  
--no-trunc: Prevents the truncation of output  
--quiet, -q: Only shows the image IDs  
```
       
Example:  
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

# Docker Pull  
```
$ docker pull [OPTIONS] NAME[:TAG|@DIGEST]  
```
This command pulls an image or a repository from a registry. Most the the images will be created on top of a base image from the Docker Hub registry, containing many pre-built images that you can pull and try without the need to define or configure your own. Use docker pull to download a particular iamge or a set of images.  
#### Options  
```
--all-tags, -a : Downloads all taged images in the repository  
--disable-content-trust : Skips the image verification. (Default is true)  
--platform : Set platform is multi-platform capable  
--quiet, -q : Suppress verbose output
```

Examples:  
Pulling an image from docker hub. (If no tag is provided, Docker Engine uses the :latest tag as default.)    
```
$ docker pull debian

Using default tag: latest
latest: Pulling from library/debian
fdd5d7827f33: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:e7d38b3517548a1c71e41bffe9c8ae6d6d29546ce46bf62159837aad072c90aa
Status: Downloaded newer image for debian:latest
```

Pulling image by digest.  
```
$ docker pull ubuntu:14.04

14.04: Pulling from library/ubuntu
5a132a7e7af1: Pull complete
fd2731e4c50c: Pull complete
28a2f68d1120: Pull complete
a3ed95caeb02: Pull complete
Digest: sha256:45b23dee08af5e43a7fea6c4cf9c25ccf269ee113168c19722f87876677c5cb2
Status: Downloaded newer image for ubuntu:14.04
```
       
# Docker Commit 
```
$ docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
This command creates a new image from a container's changes. It could be useful to commit a container's file changes or settings into a new image.  
The commit operation will not include any data actually contained in the volumes mounted inside the container.  
By default, the container being committed alonside its processes will be suspended while the image is committed, reducing the likelihood of encountering data corruption during the process of creating the commit. This can be changed by setting the --pause option to false.  
#### Options  
```
--author, -a : String that describes the author
--change, -c : Applies the dockerfile instruction to the created image
--message, -m : Details the commit message
--pause, -p : Pauses container during commit (Default is true)
```

Examples:  
Committing a container  
```
$ docker ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker commit c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker images

REPOSITORY                        TAG                 ID                  CREATED             SIZE
svendowideit/testimage            version3            f5283438590d        16 seconds ago      335.7 MB
```

Committing a container with new configurations
```
$ docker ps

CONTAINER ID       IMAGE               COMMAND             CREATED             STATUS              PORTS              NAMES
c3f279d17e0a        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            desperate_dubinsky
197387f1b436        ubuntu:12.04        /bin/bash           7 days ago          Up 25 hours                            focused_hamilton

$ docker inspect -f "{{ .Config.Env }}" c3f279d17e0a

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin]

$ docker commit --change "ENV DEBUG=true" c3f279d17e0a  svendowideit/testimage:version3

f5283438590d

$ docker inspect -f "{{ .Config.Env }}" f5283438590d

[HOME=/ PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin DEBUG=true]
```
