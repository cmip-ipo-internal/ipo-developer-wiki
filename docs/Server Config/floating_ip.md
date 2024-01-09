# Adding a floating IP on CREODIAS
To access our machine externally we first need to give it an address on how to do this. This page does just that. 

## What are floating IPs
 Floating IPs in OpenStack are public IP addresses assigned to your Virtual Machines.These allow us to host services like SSH or HTTP(s) over the Internet.

## How to assign a Floating IP?
1. Open the instances tab in Horizon
2. Use the dropdown menu and select the `Associate Floating IP` option. 
3. You may choose an address from the dropdown menu, but if it’s empty, you need to allocate an address first. Click the + icon on the right.
4. Allocate IP.

!!! warning "The IP address should be associated with a local address from the 192.168.x.x subnet. If you have a 10.x.x.x address change it to an 192.168.x.x address."

