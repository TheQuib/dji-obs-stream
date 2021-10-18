Folder: 'nginx 1.7.11.3 Gryphon'

This contains files to run an Nginx web server. It hosts 2 main services: http on port 80 (for testing) and rtmp on port 1935 (for streaming)

Use startServer.bat to start the server
  - Closing this window will not stop it

Use stopServer.bat to fully stop the server




Test to make sure the server is working:
  - Do this from a separate device
  - Go to a web browser and navigate to one of the two following things:
    - yourIPAddress
      - Ex. 192.168.0.5
    - yourHostname
      - Ex. Quinn-PC
  - If this shows a web page, the server is working as intended
  - You may have to turn off the Windows firewall to get this to work (or open the appropriate ports, 80 & 1935)


Send data to server from DJI Go 4 or DJI Fly:
  - Open the stream settings in your respective app
  - Select either "RTMP" or "Custom" (depends on the app)
  - In the URL, type either the IP or hostname, with the port 1935, and append "/live" to it
    - Ex. Quinn-PC:1935/live
    - So, the full RTMP URL would be:
      - rtmp://Quinn-PC:1935/live
    - If you want to use a custom stream key, place that after "/live"
      - ex. rtmp://Quinn-PC:1935/live/streamKey
      - You will need this same url for OBS later


View stream in OBS:
  - Open OBS
  - In a scene, create a "Media Source" source
  - In the properties of "Media Source":
    - Uncheck "Local File"
    - Set the input to the full URL of the RTMP server
      - Ex. (without stream key):
        - rtmp://Quinn-PC:1935/live
      - Ex. (with stream key):
        - rtmp://Quinn-PC:1935/live/streamKey
    - Click "OK"
    - The stream should show after a few seconds
    - Depending on the network connection from the app, and to the computer running the server, this typically has a few second delay