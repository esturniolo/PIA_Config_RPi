PIA_Config_RPi
==============


You want it, we've got it.  (i.e., ask and ye' shall receive)

If you are looking for port forwarding, the best bet is to use our application.

If you are a highly advanced user who wishes to enable port forwarding without using our state of the art application, you can still do so by utilizing our simple, SSL secured API interface:

POST to:  https://www.privateinternetaccess.com/vpninfo/port_forward_assignment
Vars:     user=username
          pass=password
          client_id=a random string that no one should be able to guess, use the same string every time
          local_ip=the 10.x.x.x IP you get assigned after connecting to the VPN

Make client_id:
osx:   head -n 100 /dev/urandom | md5 > ~/.pia_client_id
linux: head -n 100 /dev/urandom | md5sum > ~/.pia_client_id
EDIT:  linux: head -n 100 /dev/urandom | md5sum | tr -d " -" > ~/.pia_client_id 
(Thanks rcbarnes)

curl -d "user=USERNAME&pass=PASSWORD&client_id=$(cat ~/.pia_client_id)&local_ip=LOCAL_IP" https://www.privateinternetaccess.com/vpninfo/port_forward_assignment

RETURNS:
{ "port": 23423 }

You can easily make a script and call it in the 'up' section of the OpenVPN configuration.  You will need to save the JSON output and act accordingly.  Please remember that port forwarding to the router, for example, means that a port forward from the router to the device behind the router will be required.  This is for highly advanced users.

Please make the call to this API at least once every hour.

Edit: Please make sure that the HTTPS call is routed out over the OpenVPN tunnel, if you're utilizing a setup where not all traffic is routed, you will need to write your code to properly route the call over the tunnel!
