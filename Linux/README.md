# DJI-OBS-Stream - Linux

Set up information for NGINX in Linux for RTMP streaming from a DJI drone or other devices that use the RTMP protocol.


# Setting up:

This directory contains the configuration file needed to run an Nginx web server and a tutorial (below) for downloading/compiling the server.


## Prerequisites

- **Required** Open port 1935 in the server firewall for the RTMP service
- **Optional** (For testing) Open port 80 in the server firewall for the HTTP service


<br>


# Creating the Server

First thing's first... Log into your server, preferably via SSH so you can paste commands.


<br>


## Download and Install Software

### Update Repositories

```
sudo apt-get update
```

### Install Software:

```
sudo apt-get install -y git build-essential ffmpeg libpcre3 libpcre3-dev libssl-dev zlib1g-dev
```

### Clone the RTMP Module from GitHub

```
git clone https://github.com/sergey-dryabzhinsky/nginx-rtmp-module.git
```

### Download Nginx

```
wget http://nginx.org/download/nginx-1.17.6.tar.gz
tar -xf nginx-1.17.6.tar.gz
cd nginx-1.17.6
```

### Configure Nginx

```
sudo ./configure --prefix=/usr/local/nginx --with-http_ssl_module --add-module=../nginx-rtmp-module
```

### Compile Nginx

```
sudo make -j 1
sudo make install
```

### Clear out Existing Configuration File by Removing It

```
sudo rm /usr/local/nginx/conf/nginx.conf
```

### Create and Open New Configuration File

```
sudo nano /usr/local/nginx/conf/nginx.conf
```

### Paste Configuration

From the file `nginx.conf` in the directory of this `README.md` file, paste the contents

### Start Nginx

```
sudo /usr/local/nginx/sbin/nginx
```


<br>


# Make sure the server is working and send some data


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
```
rtmp {
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

```
rtmp {
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
So, with this, you would connect to rtmp://yourServer/stream/streamKey