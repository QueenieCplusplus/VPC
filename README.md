# VPC
Virtual Private Cloud

![vpc](https://cdn.qwiklabs.com/OBtRY37ZCmWiHi%2FHsG8XCSGDBfsuKk3IMJVgQscsg2E%3D)

# Core Steps:

(1) create VPC network

(2) set FW to VPC

(3) create GCE VMs

(4) explore/expose connection between VM & VPC

(5) create VM with its NICs

> start from step 1:

create VPC network.

* 1.1, in cloud console, after using IAM to login and to set project ID, then got to navigation bar to click on "Networking" >> "VPC" >> type name >> set Subnet >> set Region.

![vpc setup](https://cdn.qwiklabs.com/WEGrd5zpLB1JkgbtDzxadKeRAO%2FWkYpH5RKEfF%2Bxp%2FY%3D)

* 1.2, use cloud shell to create VPC, click on cmdline bar while in step 1.1, and type following cmd line:

          gcloud compute networks create [pvc name]
          
          gcloud compute networks create pivatenet
          
          gcloud compute networks create pivatenet --subnet-mode=custom
          
          -----------------------------------------------------------
          
          gcloud compute networks
          
          gcloud compute networks subnet create [subnet name]
          
          gcloud compute networks subnet create privatesubnet-us
          
          gcloud compute networks subnet create privatesubnet-us --network=[vpc name]
                                                                 --region=[]
                                                                 --range=[subnet range]
                                                                 
          -----------------------------------------------------------  
          
          gcloud compute networks subnet create privatenet-us
                                                --network=privatenet
                                                --region=us-central1
                                                --range=172.16.0.0.24
                                                
  * 1.3, type cmd line to list all availabe VPC.
  
          gcloud compute networks list
          
          [output]
          Name        Subnet-Mode    BGP-Routing      IPv4-Range       GW-IPv4
          
          default      auto          Regional
          
          managementㄔ   custom        Regional
          
          privatenet   custom        Regional
          
  * Tips & Attention:
  
default is auto mode networks (設定上本次課程略過), whereas, managementnet and privatenet are custom mode networks. Auto mode networks create subnets in each region automatically, while custom mode networks start with no subnets, giving you full control over subnet creation.
          
  * 1.4, type cmd line to list all available VPC subnets.  
  
           gcloud compute networks 
           
           gcloud compute networks subnets list
           
           gcloud compute networks subnets list --sort-by=NETWORK 
           // flag means subnets will be listed sorted by VPC Name
           
           
           [output]
           
           Name            Region          Network      Range
           
           deault           [略]           default
           [略]
           
           management-us    us-central1     management
           10.130.0.0/20
           
           privatesubnet-us us-central1     privatenet
           172.16.0.0/20
           
 > start from step 2:

setup VPC FW.     

* 2.1, in cloud console, click Navigation Bar >> Networking >> VPC >> VPC Networks >> Firewall >> + Create FW Rule.

           Property	     Value (type value or select option as specified)
           
              Name	               managementnet-allow-icmp-ssh-rdp
             Network	        managementnet
             Targets	     All instances in the network
             Source filter	            IP Ranges
             Source IP ranges	             0.0.0.0/0
            Protocols and ports     Specified protocols and ports, and then check tcp, type: 22, 3389; and check Other protocols, type: icmp.

 * 2.2, in cloud shell, type following cmd line to set up FW rules:
 
          gcloud compute firewall-rules 
 
          gcloud compute firewall-rules create [fw name]
                                        --network=[vpc name]
                                        --direction=[]
                                        --priority=[]
                                        --action=ALLOW
                                        --rules=icmp,tcp:22,tcp:3389
               
          
          [output]
          NAME                          NETWORK    DIRECTION  PRIORITY  ALLOW                 DENY
         privatenet-allow-icmp-ssh-rdp  privatenet  INGRESS    1000      icmp,tcp:22,tcp:3389
  
  * 2.3, to check resulting fw rules.
  
         gcloud compute firewall-rules list
         
         gcloud compute firewall-rules list --sort-by=NETWORK
         
         
 > start from step 3:

create VMs in the Region/Zone accoring with the VPC/Subnet it is located. 

* 3.1, in cloud console, click on Navigation Bar >> GCE to create VM, and add some value to the optional properties as following:


                    Property	     Value (type value or select option as specified)
                    Network	     managementnet
                    Subnetwort     managementsubnet-us

* 3.2, in cloud shell, type cmd line to create vm.

           gcloud compute
           
           gcloud compute instances
           
           gcloud compute instances create [vm name]
                                     --zone=[]
                                     --machine-type=[]
                                     --subnet=[]
                                     
            [output]
            Name           Zone         Machine Type   Preemptible   Internal IP   External IP      Status
            private-us-vm  us-central1    [略]                        172.16.0.2    35.184.221.40    running
                                                                      [自動配置]     [BGP 配置]

* Tips & Attention:

    port 22 == SSH
    
    port 3385 == RDP 


> start from step 4:

check connection using the providing IPs in step 3.2.

       ping -c 3 <Enter managementnet-us-vm's external IP here>
       
       // You are able to ping the external IP address of all VM instances, even though they are either in a different zone or VPC network. 
       
       // This confirms public access to those instances is only controlled by the ICMP firewall rules that you established earlier.
       
       ping -c 3 <Enter managementnet-us-vm's internal IP here>
       // This should not work as indicated by a 100% packet loss!
       
  
