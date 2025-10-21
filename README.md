****1. Subnet Creation****

   ****Goal:****
  
   Create a private /24 subnet in the default VPC.
   
 ****Steps:****
   
   1.  Go to VPC Dashboard in the AWS Console.
  
   2.  Click Subnets > Create subnet.
     
 ****Choose:****
     
   1.  VPC: Select your default VPC.
     
   2.  Subnet name: Private-Subnet-Dunni
     
   3.  Availability Zone:  us-east-1a
     
   4.  IPv4 CIDR block: Use a /24 block  10.0.2.0/24
     
   5.  Click Create subnet.
      
![WhatsApp Image 2025-10-21 at 21 15 15](https://github.com/user-attachments/assets/9cea1c5a-f266-4559-97c3-2d46a25dca7c)


****2. Security Hardening****

   ****Goal:****   Create and configure a security group for web servers.
   
****Steps:****

   1.  Go to EC2 Dashboard > Security Groups > Create security group.
      
   2.  Name: Web-Tier-SG
      
   3.  Description: "Allow HTTP from anywhere, SSH from My IP"

   4.  VPC: Select the default VPC

   5.  Inbound rules:

          ...HTTP | TCP | Port 80 | Source: 0.0.0.0/0

          ...SSH | TCP | Port 22 | Source: My IP 102.88.110.250/32

   6.  Click Create security group

![WhatsApp Image 2025-10-21 at 21 15 31](https://github.com/user-attachments/assets/261589c5-d215-4044-874f-37b0593f398b)


****3. Resource Association****


****Goal:****   Move Wednesday's EC2 instance to new subnet and apply new security group.

****Steps:****

****Note:**** You cannot directly move a running instance to a new subnet. You'll need to stop the instance, create an AMI, and launch a new one in the new subnet.

  1.  Identify Wednesday's EC2 instance -My-instance

  2.  Stop the instance.

  3.  Click Actions > Create Image (AMI)

  4.  After AMI is ready:

         ----Go to AMIs, select the AMI

         ----Click Launch instance

         ----Choose:

   ********Subnet: MY-DEFAULT-VPC-SUBNET

   ********Security Group: Web-Tier-SG

   ![WhatsApp Image 2025-10-21 at 21 15 53](https://github.com/user-attachments/assets/007723b1-9c22-4482-8363-b4eb126645e8)


5. Launch the instance.
   
![WhatsApp Image 2025-10-21 at 21 16 07](https://github.com/user-attachments/assets/1f877131-5f38-43c7-a3f1-8619b6de9f1a)


****4. Connectivity Verification****


   ****a)     Check HTTP (Apache) access:****

   i.   Get the Public IP of the new EC2 instance.

   ii.   Open browser and go to: http://3.91.155.158/

   iii.  the Apache test page will appear
   
![WhatsApp Image 2025-10-21 at 21 25 42](https://github.com/user-attachments/assets/6e8e31e4-d440-4c82-8e60-9d2b90260ab2)


   ****b) Test SSH restriction:****

   i.  Try SSH from an unauthorized IP (MyIP).

   ii.  Firstly, i had to change my inbound rules to shh into My-IP-102.88.110.250/32 

   iii. This means:

   ----------Only My IP can SSH in.

   ----------No one else can connect via SSH including myself

   iv.  If You See: Permission denied (publickey)
   
   ---------That means:
           
   ---------You reached the server over SSH so the IP restriction is working 
   
   v.   You should not get access.
   
![WhatsApp Image 2025-10-21 at 21 16 21](https://github.com/user-attachments/assets/431cbb49-9061-4cfa-b995-dbdce0f6ecc0)

   

