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