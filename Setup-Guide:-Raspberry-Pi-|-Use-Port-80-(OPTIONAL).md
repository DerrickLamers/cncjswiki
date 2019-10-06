Normally, CNCjs uses port 8000 to communicate between the browser and the CNCjs server (hence the *:8000* in the URL *localhost:8000*). If you browse to a URL that does not have the ":8000" part, the implied port number is 80, the standard port number for the http protocol. So if you want to access CNCjs without having to add the *:8000* to the URL, you could redirect port 80 to port 8000.  This would not work if you are already running a web server on the Pi, because that web server would already be using port 80.

You might think that you could add *--port 80* to the cncjs command line, but that does not work because port 80 (as with all ports below 1024) is privileged, so it can only be used by processes running under the special administrative user *root*.  CNCjs runs as a normal user process (typically user *pi* on a Raspberry Pi), so it will fail if you specify a port number less than 1024.

The workaround is to use the system's routing software to redirect port 80 to port 8000.  When a request comes in on port 80, the routing software forwards the request to port 8000.  Here's how:

```
# Iptables (allow access to port 8000 from port 80)
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8000

# Make Iptables Persistent
sudo apt-get install iptables-persistent -y

# How-to: Save & Reload Rules
#sudo netfilter-persistent save
#sudo netfilter-persistent reload

# How-to: Manually Save Rules
#sudo sh -c "iptables-save > /etc/iptables/rules.v4"
#sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"

# Run this if issues to reconfigure iptables-persistent
# sudo dpkg-reconfigure iptables-persistent
```
