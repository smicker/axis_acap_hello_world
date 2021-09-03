This is a hello world program for Axis cameras (an ACAP).
It will just print Hello World!!! to the app log.

# Source
This project is created from the tutorial found here:  
https://help.axis.com/acap-3-developer-guide#acap-development-requirements

# Build
You can build this in different ways:
### By using the Dockerfile
Standing in your working directory run the following commands:  
```docker build --tag <APP_IMAGE> .```  
where APP_IMAGE is the name that you want your new image to get, for example hello_world:1.0  
This will create a new image hello_world:1.0 which will contain both the acap sdk image, your project and your built files.  
You can also build for another architecture than the default armv7hf  
```docker build --build-arg ARCH=aarch64 --tag <APP_IMAGE> .```  
Then copy the result from your built image to a local directory 'build':  
```docker cp $(docker create <APP_IMAGE>):/opt/app ./build```  
Your local build folder will now contain the build files, including the .eap file.

### By building from inside the docker container (preferred)
Start a container from the image you created (hello_world) or from the main 3.3-armv7hf-ubuntu20.04 image:  
```docker run --rm -v $PWD/app:/opt/app -it hello_world:1.0```  
or  
```docker run --rm -v $PWD/app:/opt/app -it 3.3-armv7hf-ubuntu20.04```  

**axisecp/acap-sdk**            The Docker hub repostitory  
**3.3-armv7hf-ubuntu20.04**     The tag that points out which SDK version and architecture to use  
**-v \$PWD/app:/opt/app**       Mounts the host directory $PWD/app into the container directory /opt/app  
**--rm**                        Removes the container after closing it  
**-it**                         Is to run the container interactivelyx  

You should end up in a container with a prompt similar to:  
root@1e6b4f3d5a2c:/opt/app#  

Build with the command:  
```acap-build .```

Your build files will end up in your current directory, including the .eap file. And since this dir
is mounted to your local $PWD/app dir, you will also get the build files outside of your container.

# Installation
You install this on a camera by using the .eap file that is built. Select the .eap file from the camera UI.
