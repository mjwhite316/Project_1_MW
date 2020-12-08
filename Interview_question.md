Domain: Network Security
Question 1:  Faulty Firewall
Suppose you have a firewall that's supposed to block SSH connections, but instead lets them through. How would you debug it?
Make sure each section of your response answers the questions laid out below.


To make sure I understand what you're asking, the situation is that there is a firewall set up, and it should not allow any 
SSH connections to pass through, but it is. That obviously is a very serious potential threat. If exposed a threat actor 
could gain full access to the machine and not only harvest information, but change things, install malware, or even change
access credentials to lock the people out who should be able to access it. 

I had a project where I was tasked with setting up a virtual network, with 3 webservers behind a load balancer. The first 
thing I did was to put a rule in the network security settings to deny all inbound traffic no matter what. This allowed
me to set things up and make changes before exposing any of the boxes to the internet. In order to make sure only the 
proper access was allowed. I added a jump box, with the logon credetial set to my public SSH key from my machine. Obviously 
I had to also give SSH access in the network security rules, but to keep unwanted traffic out, I set the rule to only allow 
port 22 access from my specific machine, by limiting the IP addresses that were let through. From there I was able to connect
to the webservers via SSH because I was within the private network, and had set the rules to allow traffic from the jump box
to the webservers. This again used a public SSH key to increase the security. The webservers could only be accessed via SSH 
through a container on the jump box, and that public key. Any machine other than the jump box container would be denied
access because of the improper public key.

I verified that none of my machines were able to be accessed by SSH by simply trying to connect from an unauthorized source.
Luckily I got the settings correct the first time, but if I hadn't I would first assume that there was some sort of mistake
in the incoming traffic rules. Since this was in Azure, those are found in the network security group rules. I would look 
at any rule that specifically mentions port 22, or any one that allows traffic to all ports. Any rule that allows access to 
all ports should be very carefully written, and if there is a more specific rule that can be used, that would be 
preferable. If there is no reason to access these machines via SSH at all, then I would close any traffic looking for port 22.
It would also be a good idea to double check configuration files to make sure they haven't been changed to accept SSH 
connections on other ports. Another way I prevented unauthorized access was that since my web servers were behind a load balancer, 
they did not have public IP addressess. To access those machines, you had to first go through another machine on the network.
Once I changed the rules, the best way to test them is simply attempting to SSH from outside through the firewall. If the 
solution has been implemented correctly, the connection will be refused.

Of course in the world of security, there are always trade offs. I set a very specific rule that only allowed the SSH connection
to the jump box from a specific IP. That meant that If my IP address changed, which isn't uncommon, I would have to rewrite 
the rule. I overcame this when I was at home by writing a CIDR rule to allow access from any public IP assigned to my house, but 
if I were at a different location, I would have to write a new rule. 
The fact that instead of using a username and password, I used a public SSH key, made the box even more secure. The main problem 
with that however was that if I wanted to access the network from a different machine, I wouldn't be able to. Yes that makes it
more secure, but it also makes it difficult if my machine is not available, or if there are multiple people who should be able 
to access the machine. Really every situation should be examined to see where the balance between access and security lies. 
When access is required, a good IDS system would be desired. 
