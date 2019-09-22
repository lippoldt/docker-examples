This is a small docker file and composer environment to test CUDA on any Jetson fulfilling the configurations below. 

# Usage

## 1. compile deviceQuery

After setting up jetpack 4, test the CUDA version by compiling the deviceQuery:

`cd /usr/local/ccuda/samples/1_Utilities/deviceQuery`

`sudo make`

You can simply test it with:

` ./deviceQuery`

and the typical output should contain:

``` 
Detected 1 CUDA Capable device(s)
Device 0:"NVIDIA TEGRA X2"
```

Let's say your docker directory where you also put the docker and docker compose file is /home/docker, then copy the deviceQuery binary over to that location:

`cp deviceQuery /home/docker`

## 2. use docker and docker-compose

If you have not done so, create a docker user group and add your current user to this group (for the sake of not using "sudo docker")

`sudo groupadd docker`

`sudo gpasswd -a Username docker`

For the purpose of reproducability, install docker compose on the Jetson machine.

`sudo apt-get install docker-compose`

It should run through without errors. Then just build and run (at the same location as in 1):

`docker-compose build`

## 3. check output
You will need to log out and in again and then simply test by using

`docker-compose up`

The resulting output should be similar to the test on the local machine. Runtime CUDA Errors directly after installation typically occur if you have not refreshed the system.

# Configurations

Docker image base: Ubuntu 18.04 ARM64 

Size of image: 58.3MB 

Required Jetpack version: Jetpack 4.0+ (tested on 4.1 as well)
