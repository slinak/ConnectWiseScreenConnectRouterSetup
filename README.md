# ConnectWiseControlRouterSetup
 How to setup the ConnectWise Control Router service so that each service can listen on the same port.
 
 The setup was originally described on a forum that is no longer available so this repo includes the original forum post.  
 
 I have added a quick powershell script that copies the Relay service into the Router service and renames it appropriately.
Also included is a slightly updated web.config block that must be added and customized depending upon the Control server's current configuration.

        ---------Setup Instructions---------
1.  To create the new service, copy the ScreenConnect Relay service entry (HKLM:\SYSTEM\CurrentControlSet\Services\ScreenConnect Relay) within the registry and rename it to ScreenConnect Router.
2.  Reboot the server.
3.  Add the configuration blocks for the Router service to the web.config file, just below the opening <configuration> block.
4.  Modify the forwardPayload actions to correctly match the current WebServerListenUri and RelayListenUri ports. (Example below)
5.  Save the web.config file.


Web.config Setup Example:
This example is for a default Control server without HTTPS, the Web server is listening on 8040 and the Relay on 8041.  
Web requests that come in on either 80 or 443 are forwarded to the correct port but HTTPS is not applied because the server is not serving those requests.

<configSections>
  <section name="screenconnect.routing" type="ScreenConnect.RoutingConfigurationHandler, ScreenConnect.Server" />
 </configSections>
<screenconnect.routing>
  <listenUris>
   <listenUri>tcp://+:80/</listenUri>
   <listenUri>tcp://+:443/</listenUri>
  </listenUris>
  <rules>
   <rule schemeExpression="http" actionType="issueRedirect" actionData="http://$HOST:8040/" />
   <rule schemeExpression="ssl" actionType="forwardPayload" actionData="http://localhost:8040/" />
   <rule schemeExpression="relay" actionType="forwardPayload" actionData="relay://localhost:8041/" />
  </rules>
 </screenconnect.routing>
