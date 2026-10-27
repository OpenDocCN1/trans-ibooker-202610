Now that we know more about the Kubernetes service types, let’s continue. To create the services using these converted files, execute the following commands on the root of the AWS/ELK folder:

```
kubectl apply -f <service>.yaml <deployment>.yaml
```

We prefer to execute this command individually to see that everything is created successfully, but you can also create all the services simultaneously. To do this, you need to concatenate all the YAML files with the -f argument as in the previous command. For example, to create the configuration server and view the pod status and the logs, we need to execute the following commands:

```
kubectl apply -f configserver-service.yaml,configserver-deployment.yaml
kubectl get pods
kubectl logs <POD_NAME> --follow
```

To test that our service is up and running, we need to add a few rules to our security group to allow all the incoming traffic from the node ports. To do so, let’s execute the following command to retrieve the security group ID:

```
aws ec2 describe-security-groups --filters Name=group-name,Values="*eksctl-ostock-dev-cluster-nodegroup*" --query
    "SecurityGroups[*].{Name:GroupName,ID:GroupId}"
```

With the security group ID, let’s execute the following command to create an inbound rule. You can also do this in the AWS Management Console by going to the security group and creating a new inbound traffic rule like this:

```
aws ec2 authorize-security-group-ingress --protocol tcp --port 31000
--group-id [security-group-id] --cidr 0.0.0.0/0
```

To get the external IP, you can execute the kubectl get nodes -o wide command. This command shows you output similar to the one shown in figure 12.16.

![](bbox=[0.146, 0.672, 0.894, 0.731])

Now you can open the following URL in your browser: http:<node-external-ip>:<NodePort>/actuator. In case you need to delete a pod, service, or deployment, you can execute one of the commands listed here:

```
kubectl delete -f <service>.yaml
kubectl delete -f <deployment>.yaml
kubectl delete <POD_NAME>
```