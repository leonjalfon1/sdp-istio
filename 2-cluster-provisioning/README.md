# CLUSTER PROVISIONING
---

 - In this section we will create an eks cluster using eksctl
 
- Content:

   - Create an EKS cluster

---

## Create an EKS cluster

 -  To create an empty cluster, run:
```
eksctl create cluster -f ./eks-cluster.yaml
```

---

## Notes

 - You can inspect the cluster nodegroups by run:
```
eksctl get nodegroup --cluster=leonj-sdp-istio --region=ap-southeast-1
```

 - You can remove a cluster nodegroup by run:
```
eksctl delete nodegroup <nodegroup-name> --cluster=leonj-sdp-istio --region=ap-southeast-1
```

 - To scale a node group you can use:
```
eksctl scale nodegroup --cluster=leonj-sdp-istio --nodes=5 --name=linux-nodes
```

 - You can delete the cluster by using the command below:
```
$ eksctl delete cluster --name=leonj-sdp-istio --region=ap-southeast-1
```

---