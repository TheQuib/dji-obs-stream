# DJI-OBS-Stream
NGINX Server for Windows that will work out of the box to stream video from a DJI Drone

# Setting up:
This contains files to run an Nginx web server. It hosts 2 main services: HTTP on port 80 (for testing) and RTMP on port 1935 (for streaming)

## Prerequisites
 - **(Recommended)** Open at least port 1935 (80 for testing) in the Windows Firewall for the RTMP service to leave the computer
 - **(Not Recommended)** Disable the Windows Firewall altogether
   - *I am not responsible for any damage done by opening up the entire firewall*

<br>

## Start the server
Use startServer.bat to start the server
 - Closing this window will not stop it

## Stop the server
Use stopServer.bat to stop the server

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
   - Depending on the network connection from the app, and to the computer running NGINX, this can take longer