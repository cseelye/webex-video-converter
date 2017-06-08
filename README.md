# webex-video-converter
The WebEx video converter is notoriously difficult to get working on a modern linux distro. This containerizes the tool and gives you a helper script to assist in running it.

For simplicity, the file to be converted must be somewhere under your home directory. The helper script mounts your home directory into the same path in the container - if your home is in /home/cseelye on your host, it will be in /home/cseelye in the container as well.  This means that any paths you use for files will work when passed into the container.

## Usage ##
The arguments passed to the convert script are passed verbatim to the nbr2mp4 script in the container.

```Usage: ./nbr2mp4 SOURCE [MP4_DIRECTORY] [FPS]

SOURCE: The name of the source Advanced Recording Format file (.arf)
that you want to convert. Specify a path name if the file is in a
directory other than the current directory.

MP4_DIRECTORY: The directory for the MP4 output file. If MP4_DIRECTORY
is not specified, the MP4 output file will be generated in the same
directory as the SOURCE file.

FPS: The number of frames per second, which should be between 3 and 10,
default = 5.
```

### Security Implications ###
Webex, in their infinite wisdom, has made these tools dependent on X11. The helper script exports your DISPLAY variable into the container, and runs the container with --net=host and --ipc=host, so the container shares the host network namespace and can access the X11 socket and shared memory. Note that this removes some of the security/isolation of the container so it has much more access to your host than a typical container.
