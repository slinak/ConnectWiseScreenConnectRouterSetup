# ConnectWiseControlRouterSetup
 How to setup the ConnectWise Control Router service so that each service can listen on the same port.
 
 The setup was originally described on a forum that is no longer available so this repo includes the original forum post.  
 
 I have added a quick powershell script that copies the Relay service into the Router service and renames it appropriately.
Also included is a slightly updated web.config block that must be added and customized depending upon the Control server's current configuration.
