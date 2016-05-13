#### psawsvpcop
#####3
######starting wizard
for 10.0.0.0/16 first2=network address, last2 = device address.  
######3
vpc are per region.  
IP CIDR 10.1.0.0/16  
public 10.1.0.0/24  
Availability Zone:us-east-1a

private 10.1.1.0/24  
Availability Zone:us-east-1a(same)  

instance type: m1.small
######6 Revireing your vpc
vpc->subnets->choose public subnet->subnet actions->modify aa public IP->enable

#####4
######5build two subnets
10.0.0.0/16--10.0.1.0/24(pub)--10.0.2.0/24(pri)(same az us-east-1)

#####5
######3
create a new route table for public subnet: add destination 0.0.0.0/0, target=igw  
######8 add NAT instance
search NAT HVM in marketplace(community ami)  
create security group: add HTTP, port80,source CustomIP, address=private(10.0.2.0/24)  
add HTTPS, port443,source CustomIP, address=private(10.0.2.0/24)  
create a route table for private subnet, add a route, 0.0.0.0/0, target=nat instance
#####6
######4 implementing secirity groups(stateful)
create->port2375 source=10.0.2.0/24 (connect to nat instance)  
change security groups in EC2->network instance
######5 NACL
explicit DENY explicit ALLOW stateless  
one NACL per subnet. rule processing stops upon match.20 rules per NACL  
######6
when creating a vpc,a nacl will be created

#####8
######2 vpc peering
Two vpc must be in same region. Do not overlap CIDR blocks.Ex  
vpc1:cidr 10.0.0.0/16  
vpc2:cidr 172.1.0.0/16
######3 update routing table (1:20)
Add to first VPC's public subnet. destination:vpc2CIDR  target: pcx 
Add to second VPC's public subnet. destination:vpc1CIDR  target: pcx 
