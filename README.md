Download Link: https://assignmentchef.com/product/solved-embsys110-assignment-6-iot-traffic-light
<br>
In this assignment, students will integrate the WiFi module to the project such that:

<ol>

 <li>The traffic light status is reported to a server application.</li>

 <li>The traffic light can be controlled by a server application.</li>

</ol>

<h1>2 Reference</h1>

<ol>

 <li>ST UM2183 X-NUCLEO-IDW04A1 User Manual.pdf</li>

 <li>STSW-IDW004 WiFi Hands On Training.pdf</li>

 <li>ST UM2114 SPWF04Sx AT Command Reference.pdf</li>

</ol>

<h1>3 Basic Setup</h1>

<ol>

 <li><em>Carefully</em> plug in the following extension modules in the exact order onto your Nucleo board:

  <ul>

   <li>X-NUCLEO-IKS01A1 (IMU sensor).</li>

   <li>X-NUCLEO-IDW04A1 (WiFi).</li>

   <li>LCD module.</li>

  </ul></li>

</ol>

Please ensure you align the pins correctly and do not bend any pins.

Note – In this assignment we are only using the LCD and WiFi modules.

<ol start="2">

 <li>Download the compressed project file (platform-stm32f401-nucleo_assignment6.tgz) from the Assignment 6 course site.</li>

</ol>

Place the downloaded tgz file in your VM under <strong>~/Projects/stm32</strong>.

<ol start="3">

 <li>Move your existing project folder to a backup folder, e.g.</li>

</ol>

mv platform-stm32f401-nucleo platform-stm32f401-nucleo.bak

Note – You may want to pick another backup folder name if the one shown above already exists.

<ol start="4">

 <li>Decompress the tgz file with:</li>

</ol>

tar xvzf platform-stm32f401-nucleo_assignment6.tgz

The project folder will be expanded to <strong>~/Projects/stm32/platform-stm32f401-nucleo</strong>.

<ol start="5">

 <li>Launch Eclipse. Hit F5 to refresh the project content.</li>

</ol>

Or you can right-click on the project in Project Explorer and then click “Refresh”.

<ol start="6">

 <li>Clean and rebuild the project. Download it to the board and make sure the LCD shows the traffic light graphics.</li>

 <li>If you have not installed UMLet, please refer to the Setup notes in the previous assignment.</li>

</ol>

<h1>4 Tasks</h1>

<h2>4.1 Install Node.js</h2>

Node.js is a popular software platform for developing server applications with Javascript. To complete this assignment, you need to first install Node.js on your VM by following the steps below:

<ol>

 <li>Install nvm – Node Version Manager.</li>

</ol>

Open a Terminal, go to your home directory (“cd ~/”) and type:

<h3>sudo apt-get update sudo apt-get install curl</h3>

(Note – if you see an error about resource temporarily unavailable, type this to remove a lock file: sudo rm /var/lib/dpkg/lock) <strong>curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash</strong>

(Note – The above is one line.) <strong>source ~/.profile</strong>

<h3>2. Type: nvm ls-remote</h3>

It should list out available versions.

<ol start="3">

 <li>Type:</li>

</ol>

<h3>nvm install 11.14.0 nvm alias default 11.14.0</h3>

It installs the specified node version (must <em>not</em> be 12.x.x). Type “<strong>node –version</strong>” to check version. If you see “v11.14.0”, congratulations for having successfully installed Node.js.

<h2>4.2 Install TCP Server/Client Apps</h2>

<ol>

 <li>Create a subdirectory named “node” in ~/Project:</li>

</ol>

<h3>cd ~/Projects mkdir node</h3>

<ol start="2">

 <li>Download the following compressed files from the Assignment folder on the course website and put them in the “node” subdirectory created in step (1):</li>

</ol>

<strong>statenode-tcpclient.tgz statenode-tcpserver.tgz</strong>

<ol start="3">

 <li>Decompress the tgz files under the “node” subdirectory:</li>

</ol>

<strong>cd ~/Projects/node tar xvzf statenode-tcpclient.tgz tar xvzf statenode-tcpserver.tgz </strong>4. Setup the TCP server application: <strong>cd ~/Projects/node/statenode-tcpserver npm install</strong>

It is okay to ignore any warnings (in yellow). You shouldn’t see any errors (in red) though.

<ol start="5">

 <li>Setup the TCP client application <em>in another Terminal window</em>: <strong>cd ~/Projects/node/statenode-tcpclient npm install</strong></li>

</ol>

You are now all set with the Node apps.

<h2>4.3 Test TCP Server/Client Apps</h2>

<ol>

 <li>In the server Terminal (assuming it is in <em>statenode-tcpserver</em> folder), type: <strong>node src/main.js</strong></li>

</ol>

You should see some log messages ending with:

“&lt;LOG&gt; TcpSrv: Enter started”

<ol start="2">

 <li>In the client Terminal (assuming it is in <em>statenode-tcpclient</em> folder), type:</li>

</ol>

<h3>node src/main.js</h3>

You should see some log messages ending with:

“&lt;LOG&gt; TcpClient: SockOnMessage:  Received OK”

<ol start="3">

 <li>This shows that your TCP server is running correctly and is ready to get connected with your Nucleo board.</li>

</ol>

<h2>4.4 Network Settings of VM</h2>

To ensure data can be transferred between the VM and external device (i.e. the Nucleo board), we need to check a few settings of our VM

<ol>

 <li>Ensure “Bridged Adapter” (rather than “NAT”) is selected for the VM network adapter.</li>

 <li>Ensure the port number 60002 is enabled in the firewall (in case it is enabled):</li>

</ol>

<strong>sudo ufw allow 60002</strong>

<h2>4.5 WiFi Module Setup</h2>

The purpose of this section is to setup the WiFi parameters of your X-NUCLEO-IDW04A1 module so that it will connect to your home WiFi router (to which your VM is also connected).

<ol>

 <li>First run the firmware from the Eclipse project platform-stm32f401-nucleo.</li>

 <li>In minicom, type the command “wifi interact” to enter interactive mode.</li>

</ol>

In interactive mode, the command console passes-through to the UART port of the WiFi module. That is, what you type on the console is sent to the WiFi module directly and its output is displayed on the console.

<ol start="3">

 <li>Follow the instructions on page 75, Section 4.2.1 of STSW-IDW004 WiFi Hands On Training.pdf to configure your WiFi module.</li>

</ol>

<h3>AT+S.WIFI=0 AT+S.SSIDTXT=&lt;ssid&gt; AT+S.SCFG=wifi_wpa_psk_text,&lt;password&gt;</h3>

<strong>AT+S.SCFG=wifi_priv_mode,&lt;mode&gt; </strong>where &lt;mode&gt; = 0 for none, 1 for WEP, <strong>2 for WPA/WPA2</strong>

<strong>AT+S.SCFG=wifi_mode,&lt;mode&gt; </strong>where &lt;mode&gt; = <strong>1 for STA</strong>, 3 for MiniAP

<strong>AT+S.WIFI=1 AT+S.WCFG</strong>

You can check the connection status including the assigned IP address by typing:

<strong>AT+S.STS</strong>

You can check your WiFi settings by typing:

<h3>AT+S.GCFG</h3>

For example, if you want your WiFi module to connect to your AP with ssid = “My Router AP”, type these:

<strong>AT+S.WIFI=0 AT+S.SSIDTXT=<em>My Router AP </em>AT+S.SCFG=wifi_wpa_psk_text,<em>my_password</em></strong>

<h3>AT+S.SCFG=wifi_priv_mode,2 AT+S.SCFG=wifi_mode,1 AT+S.WIFI=1 AT+S.WCFG</h3>

Alternatively, you may configure your WiFi module as an mini-AP (e.g. named “My Mini AP”), to which your PC/laptop  connects directly (without both connected to a WiFi router):

<h3>AT+S.WIFI=0 AT+S.SSIDTXT=<em>My Mini AP </em>AT+S.SCFG=wifi_priv_mode,0 AT+S.SCFG=wifi_mode,3 AT+S.WIFI=1 AT+S.WCFG</h3>

<ol start="4">

 <li>Once your WiFi module is properly configured you can exit the interactive mode by hitting CTRL-C, or simply resetting the Nucleo board. This will bring the firmware back to the command mode. Since the configuration has been saved to flash (of the WiFi module), you shouldn’t need to configure it again unless you want to change the parameters.</li>

</ol>

<h2>4.6 Testing WifiSt</h2>

WifiSt (under src/Wifi/WifiSt) is the active object responsible for interfacing with the STMicro’s WiFi module. It is partially implemented to demonstrate basic connectivity.

It handles the following events:

<ol>

 <li>WIFI_CONNECT_REQ – Connects to the TCP server listening at the specified IP address and port number. The IP address should be that of your VM running the Node.js TCP server. You can check your VM’s IP address by typing “<strong>sudo ifconfig</strong>“. The port number used by the TCP server is fixed to be <strong>60002</strong>.</li>

 <li>WIFI_DISCONNECT_REQ – Disconnects a previously established TCP connection.</li>

 <li>WIFI_SEND_REQ – Sends the data carried by this event over a previously established TCP connection.</li>

 <li>UART_IN_DATA_IND – Handles the UART data received from the WiFi module. These data are ASCII characters which can either be:

  <ol>

   <li>AT response strings starting with “<strong>AT-S”</strong></li>

  </ol></li>

</ol>

See ST UM2114 SPWF04Sx AT Command Reference.pdf.

<ol>

 <li>Asynchronous messages starting with “<strong>+WIND:&lt;WIND ID&gt;”</strong></li>

</ol>

See page 57 of ST UM2114 SPWF04Sx AT Command Reference.pdf.

Note that when the TCP server sends a message to the WiFi module, the module will first notify the Nucleo firmware via an asynchronous message “<strong>+WIND:55″. </strong>Then the Nucleo firmware will issue an AT command “<strong>AT+S.SOCKR=0,
r</strong>” to request to read all the received data from the module. Then the module will reply with an AT response starting with “<strong>AT-S.Reading</strong>“, followed by the message payload, and ending with “<strong>AT-S.OK</strong>“.

This is an example:

1122669 WIFI_ST(14): Received: <strong>+WIND:55</strong>:Pending Data::0:31:31

1122690 WIFI_ST(14): Received: <strong>at+s.sockr=0,</strong>

<strong>AT-S.Reading</strong>:31:31

<em>this is my hello world message</em>

<h3>AT-S.OK</h3>

Currently WifiSt simply prints out any received messages to the console, like the ones shown above. You will have to parse these received messages and appropriate actions (see below.)

You can test out the above functions with the following console commands. See the help text for details:

<h3>1. wifi connect 2. wifi send 3. wifi disconnect</h3>

To test data reception, just type a message in the Node.js TCP server terminal window (under <strong>~/Projects/node/statenode-tcpserver</strong>). You should see the same message printed on the command console of the Nucleo board.

<h2>4.7 Requirements</h2>

The current design of the <strong>WifiSt</strong> active object, together with the “wifi” console commands demonstrates basic functions using the WiFi module. Now you need to complete the design to meet the following requirements:

<ol>

 <li>Send traffic light status to the Node.js TCP server which will print it out on the Terminal.

  <ol start="4">

   <li>Refer to the <strong>Conn() </strong>and <strong>Send()</strong> function in WifiStCmd.cpp (under src/Wifi/WifiSt) for an example of how to connect to the Node.js TCP server and send message to it. You can hardcode the IP address of your server and 60002 as the port number in your code (see Section 4.6 for details).</li>

   <li>Modify Traffic.cpp to connect to the TCP server upon start up (e.g. entry to the Started state). Note you will need to add an include file for “WifiInterface.h” at the top of the file.</li>

   <li>Modify Lamp.cpp (under src/Traffic/Lamp) to send a message (text string) to the TCP server whenever the traffic light is updated (similar to how you call the Draw() function in the previous assignment). Note you will need to add an include file for “WifiInterface.h” at the top of the file. You can design your own message format.</li>

  </ol></li>

 <li>Handle commands received from the Node.js TCP server to control the traffic light direction (NS vs EW).

  <ol start="4">

   <li>Parse the data received from the WiFi module in the Normal state of WifiSt.cpp upon UART_IN_DATA_IND. See Section 4.6 item 4 for details of the data format. Your goal is to extract the message payload sent by the TCP server, i.e. to filter the AT command/response wrappers added by the WiFi module.</li>

   <li>For the purpose of this assignment your parser does not need to be perfect. You can assume the entire message payload always arrives together in a single AT response. You can design your own message format to make your parsing easier. For example you can have a message “TRAFFIC NS” to represent a car request along the North South direction. You can use common C string library functions, such as strstr(), for parsing. Be careful not to read beyond the end of a received message.</li>

   <li>Once a server message is identified in <strong>WifiSt</strong>, you can send the corresponding event (TRAFFIC_CAR_NS_REQ or TRAFFIC_CAR_EW_REQ) to <strong>Traffic</strong>, in a similar way as you send events from <strong>System</strong> to <strong>Traffic</strong> upon button press and hold in the previous assignment. The <strong>Traffic</strong> active object does not need to be modified.</li>

  </ol></li>

</ol>