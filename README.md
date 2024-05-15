# A Practical Guide to EKS

In this repository you will find all the assets required for the course `A Practical Guide to Amazon EKS`, by A Cloud Guru.


## Bookstore application

This solution has been built for for explaining all the concepts in this course. It is complete enough for covering a real case of microservices running on EKS and integrating with other AWS Services.

> You can find in [here](_docs/api.md) the documentation of the APIs.

## 1. Introduction 
To get the bookstore application running locally run `docker compose up` from the root of the project to run the bookstore app with Docker.

To setup the EKS cluster follow the instructions below. 

1. Use the DynamoDB CloudFormation templates in each of the api's `infra/cloudformation` directories to build the DynamoDB table for each api. 
2. Use `eksctl` and the reference files in [Infrastructure/eksctl](./Infrastructure/eksctl/) to build the EKS cluster in AWS. 

## 2. Networking and EKS 
In this section we're going to be configuring various network aspects of EKS such as DNS, ACM, CNI etc. 

### Go Private, Go Secure, Go OpenVPN
Implementing OpenVPN gives end users access to private ec2 instances from outside of the VPC. This protects against unauthorised access from the internet. 

To implement OpenVPN follow the following steps...
1. In the AWS Console search for and click on `MarketPlace`
2. When you're in the marketplace, click on `Discover products` and search for `OpenVPN inc`
3. Subscribe to the `OpenVPN Access Server` which is a BYOL model. 
4. Now, back in the `Managed subscriptions` page of the Marketplace, select `OpenVPN Access Server` and then click on `Launch new instance` 
5. Select the region where you're EKS cluster is hosted and then copy the AMI-ID at the bottom of the form.
6. Update the [openvpn.yaml](./Infrastructure/cloudformation/openvpn/openvpn.yaml) with the Parameter `NodeImageId: Default: <AMI_ID>`
7. In the AWS Console create a new cloudformation stack and reference the above file. Fill in the parameters and run the stack. 
8. Once the server is deployed, copy the security group ID associated with the OpenVPN server and update the `ClusterSharedNodeSecurityGroup`, attached to the worker nodes, to allow all traffic from the OpenVPN security group. 
9. Browse to the public IP of the OpenVPN server and login with the username and password that was set in the previous CloudFormation template
10. Download the client, and if necessary the user profile. Then use the client and the user profile to connect to the server. 

To allow OpenVPN to connect to a secondary CIDR follow the steps below. Note, this is needed for the upcoming section around CNI. 
1. Allow SSH traffic to the OpenVPN server (locked down to your IP)
2. SSH onto the OpenVPN server. For example, `ssh -i "./.ssh/openvpn-server.pem" openvpnas@<instancedns or ip>`
3. Run the following command to add a route to the secondary CIDR `sudo /usr/local/openvpn_as/scripts/sacli --key "vpn.server.routing.private_network.2" --value "100.64.0.0/16" ConfigPut`
4. Restart the server with `sudo /usr/local/openvpn_as/scripts/sacli stop` and then `sudo /usr/local/openvpn_as/scripts/sacli start`
5. Finally remove the SSH security rule attached to the OpenVPN server