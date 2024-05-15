The yaml files stored in this directory are used as a reference for the `eksctl` command line tool. For example, to initialise a new cluster we run...
```
eksctl create cluster -f .\01-initial-cluster\cluster.yaml
```
For more info around `eksctl` see [https://eksctl.io/](https://eksctl.io/)