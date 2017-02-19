# my-eclipse
## Dockerfile for running Eclipse with my settings anywhere
### Run on Windows
	1. Install Cygwin
	2. Run XWin :0 -listen tcp -multiwindow in Cygwin
	3. docker build -t my-eclipse .
	4. docker run -ti --rm -e DISPLAY=<your-ip>:0 my-eclipse
### Run on Linux
	1. xhost +
	2. docker build -t my-eclipse .
	3. docker run -ti --rm -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix  my-eclipse