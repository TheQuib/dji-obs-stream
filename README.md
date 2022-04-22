# DJI-OBS-Stream

Nginx Server for Windows and Linux with RTMP support. Perfect for streaming from a DJI drone or other devices that use the RTMP protocol.


# Setting up:

This contains files/tutorial to run an Nginx web server in Windows (Files) and Debian/Ubuntu Linux (tutorial). It hosts 2 services: HTTP on port 80 (for testing) and RTMP on port 1935 (for streaming)


## Prerequisites

- **Required** Open port 1935 in the server firewall for the RTMP service
- **Optional** (For testing) Open port 80 in the server firewall for the HTTP service


<br>


## Usage

Please refer to the respective directory of your operating system of choice. Each has its own `README.md` file to get you going.


<br>


## Test to make sure the server is working

*You really only need to do this the first time* <br>
*Do this from a separate device to get the most accurate results*
 - Open a web browser
 - Navigate to one of the following URLs:
   - *yourIPAddress*
     - *Ex.* 192.168.0.5
   - *yourHostname*
     - *Ex.* Stream-PC
 - If this shows a web page, the server is working as intended
 - If this doesn't work, check your firewall to make sure the ports are open


 <br>


 ## Send data to server from DJI Go 4 or DJI Fly
 
 - Open the stream seetings in your respective app
 - Select either "RTMP" or "Custom"
   - *This selection depends on your app*
 - In the URL, type either the IP or hostname, with te port 1935, and append /live to it
 - Ex. Stream-PC:1935/live
 - So, the full RTMP URL would be:
   - rtmp://Stream-PC:1935/live
     - *Make sure you save this for later*
 - If you want to use a custom stream key, place that after "/live"
   - Ex. rtmp://Stream-PC:1935/live/*streamKey*
     - You can make *streamKey* any alphabetical character you want
     - *Make sure you save this for later*


 <br>


 ## View stream in OBS
 
*Do this on the server*
 - Open OBS
 - In a scene, create a "Media Source" source
 - In the properties of "Media Source":
   - Uncheck "Local File"
   - Set the input to the full URL of the RTMP server
     - Ex. (without stream key)
       - rtmp://Stream-PC:1935/live
     - Ex. (with stream key)
       - rtmp://Stream-PC:1935/live/streamKey
   - Click "OK"
 - The stream should show after a few seconds
   - Depending on the network connection from the app, and to the computer running Nginx, this can take longer


<br>


## Extra Info

If you would like to change the URL to use something other than `/live`, edit the `Nginx.conf` file under `/ServerFiles/conf/` directory.
<br>
The default configuration for the RTMP application is:
<br>

```rtmp {
    server {
        listen 1935;
        application live {
            live on;
            interleave on;
            record off;

        }
    }
}
```

<br>

If you change the name of the application, i.e. where it says `application live {`, let's say to `stream`, it will use `/stream` instead.

<br>
<br>

So, the application for `/stream` in `Nginx.conf` file would look like this:

<br>

```rtmp {
    server {
        listen 1935;
        application stream {
            live on;
            interleave on;
            record off;

        }
    }
}
```
<br>

So, with this, you would connect to `rtmp://yourServer/stream/streamKey`


<br>


## For the best results...

- I would recommend opening a hotspot on the computer running the server if possible. 
  - This can be easily done in Windows with the built-in hotspot
  - In some Linux distros, this is built in. Look up the features your distro has to do this there