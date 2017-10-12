# DNS-VPN

DNS-VPN is a Mikrotik RouterOS script for auto-updating your IPsec policies and peers with the resolved IP of a domain name.

  - Dynamic IP site-to-site VPN tunnels
  - As easy as specifying a comment
  - Fast enough to run every few seconds

## Example
You want to create a [site-to-site IPsec VPN link](https://wiki.mikrotik.com/wiki/Manual:IP/IPsec#Site_to_Site_IpSec_Tunnel) between Mikrotik routers. Unfortunately, one or both sides have a dynamically assigned IP address. You have a domain name, `vpn.example.com`, that you keep updated with the current IP address of the VPN endpoint, but Mikrotik doesn't let you use domain names when specifying IPsec policies or peers addresses.

1. Add the following comment to an IPsec policy and peer
     `dns-vpn,vpn.example.com`
2. Add the script in `dns-vpn.txt` to your RouterOS router's [script repository](https://wiki.mikrotik.com/wiki/Manual:Scripting#Script_repository).
3. [Set up a schedule](https://wiki.mikrotik.com/wiki/Manual:System/Scheduler) to run the script every few seconds or so.
 
That's it. The domain name you specify in the comment will be resolved and the IP will be used to update the IPsec Peers and Policies that you decorate with this special comment.

## What it Modifies
*"If I put this script on my router, what is it gonna f*** up?"*

For each **commented** [IPsec Peer](https://wiki.mikrotik.com/wiki/Manual:IP/IPsec#Peer_configuration), this script will change the `address` property.
For each **commented** [IPsec Policy](https://wiki.mikrotik.com/wiki/Manual:IP/IPsec#Policy), this script will change the `sa-dst-address` property.

## Requires
 - Only tested on RouterOS 6.40.4, [but any version after6.2 should work](https://wiki.mikrotik.com/wiki/Manual:Scripting#Function).
 - A domain name that you keep up to date with IP address of your IPsec VPN endpoint.
 - The script policy needs to have at least `read`, `write`, `policy`, and `test` [permissions](https://wiki.mikrotik.com/wiki/Manual:Router_AAA#Properties).