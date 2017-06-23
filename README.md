# Nat-wlan0-to-eth0
Forward Internet From wlan0 to eth0 in Debian

## To Target Board(It is only eth0) 

In our target board not wlan0 interface only eth0 interface is there 

so set ip address to eth0 (If you need sudo premission) (192.168.1.20 is your laptop ip)

````
sudo ifconfig eth0 192.168.1.20 netmask 255.255.255.0
sudo route add default gw 192.168.1.30
````

## To laptop(It is wlan0 and eth0)

It need sudo privilege

````
sudo /etc/init.d/networking restart
````
above command is optional

````
sudo nano /etc/network/interfaces
````
and add the line

````
allow-hotplug wlan0
````
Is using nano ctrl-X,Y,Enter to save this

````
sudo ifconfig eth0 192.168.1.30 netmask 255.255.255.0
````

````
iptables -A FORWARD -o wlan0 -i eth0 -s 192.168.1.0/30 -m conntrack --ctstate NEW -j ACCEPT
iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
````
Next ping in target board it is working


Reference
https://www.raspberrypi.org/forums/viewtopic.php?t=7323&p=102166
https://gist.github.com/tzermias/5408466x
