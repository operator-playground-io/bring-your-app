###1. Login to image repository

If you have used private image in the worklaods, login to the repository

```copy
docker login <repo-name>
```



###2. Application name

Enter the same name of the application again.

```copy
export AppName=<REPLACE APP NAME>
```



###3. Deploy all the Kubernetes resources

```execute
cd /home/student/code-server/build-k8s-resources/ && kubectl apply -f $AppName -n $AppName
```



###4. Check the status of the resources

```execute
kubectl get all -n $AppName
```

