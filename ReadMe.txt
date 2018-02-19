#0>. if port 9000 os open then run chat php server by following command:

   php -q /var/www/html/chat/server.php
   
   NOTE: url: http://chat.local/

#1>. Check port is open or not:

   netstat -an | grep PORTNUMBER | grep -i listen
   eg: netstat -an | grep 9000 | grep -i listen
   
#2>. How to open port:
 
   a>. Connect via SSH and list current IPtables
        $ sudo iptables -L
        or 
        $ sudo iptables -nvL (For more detail)
        
   b>. Flush Unwanted Rules
        $ sudo iptables -F
        
   c>. Add Firewall Rule
       
       i>.The first firewall rule you need to add is the following one
       $ sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
       
       ii>. This basically tells your firewall to accept your current SSH connection.
             The next step is to allow traffic on your loopback
              interface and to open some basic ports like 22 for SSH and 80 for HTTP.
              
        $ sudo iptables -A INPUT -i lo -j ACCEPT
        $ sudo iptables -A OUTPUT -o lo -j ACCEPT
        $ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
        $ sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
        $ sudo iptables -A INPUT -p tcp --dport 9000 -j ACCEPT
