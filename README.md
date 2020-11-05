# VPC
Virtual Private Cloud

![vpc](https://cdn.qwiklabs.com/OBtRY37ZCmWiHi%2FHsG8XCSGDBfsuKk3IMJVgQscsg2E%3D)

# Core Steps:

(1) create VPC network

(2) create GCE VMs

(3) explore/expose connection between VM & VPC

(4) create VM with its NICs

start from step 1:

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
          
          management   custom        Regional
          
          privatenet   custom        Regional
          
                                                                
          
          
