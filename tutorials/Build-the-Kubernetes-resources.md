###1. Clone the k8s templates

```execute
cd /home/student/code-server/ && git clone https://github.com/operator-playground-io/build-k8s-resources.git
```

###2. Create pre-requisite directory

```execute
mkdir -p /home/student/code-server/build-k8s-resources/$AppName
cd /home/student/code-server/build-k8s-resources/$AppName
```

###3. Use the Developer Dashboard to create k8s resources

![codestructure](_images/IDE.png)

Map the kuberntes resources to your applications artifacts as described below.

###4. Create the yamls for configuration

Map the configuration files to **Configmap**.

The Key Value pairs can be replaced by the file name and the entire file contents as below

```copy
data:
	application.properties: |
        connection.types=ftp,scp
        connection.timeout=5 sec 
```

The Key Value pairs can be replaced by individual properties from your configuration file as given below.

```copy
data:
  maximum.retries: "3"
  log.level: "DEBUG"
```



###5. Create secrets for sensitive information

Map the files containing sensitive information or passwords to **Secret**.

The Key Value pairs can be replaced by individual properties from your configuration file as given below.

```copy
stringData:
  username: YWRtaW4=
  password: MWYyZDFlMmU2N2Rm
```

Value should be Base64 encode.

```copy
echo -n <value> | base64
```



###6. Create storage

If the application involves saving data in a Storage then create a **PersistentVolume (PV) ** and a **PersistentVolumeClaim (PVC)**

Name the storageClassName to any name, the same name should be used in PV and the corresponding PVC.

Set the accessModes to one or mroe of - ReadWriteOnce, ReadOnlyMany, ReadWriteMany, ReadWriteOncePod

In PV acclocate storage capacity of the volume in Gi or Mi. Here we will be creating a PV of type *hostPath* so the storage will be created on the Node's filesystem under path /mnt/data.

```copy
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

In PVC set the storage requried by your application in Gi or Mi. This size should be same as or less that the size allocated to the PV. 

```copy
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

###7. Create workloads 

Map your application images to any of the below workloads depending on the functionality of the image.

| Workload type | Functionality                                        |
| ------------- | ---------------------------------------------------- |
| Pod           | Basic unit; can have one or more containers          |
| Deployment    | Manage a stateless application; run one or more pods |
| Statefulsets  | Manage a stateful application; run one or more pods  |
| Job           | Run application once and stopping                    |
| Cronjob       | Run a job according to a schedule                    |

**All of the workloads** will require the *image* and *label values*

If you have created a configmap in the above steps, one of the ways to use it is to mount it as a volume to the workload as in the below example:

```copy
spec:
  containers:
    - name: test-container
      image: <IMAGE>
      volumeMounts:
      - name: config-volume
        # Provide the path as per application
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        # Provide the name of the ConfigMap containing the files you want
        # to add to the container
        name: application-configmap
  restartPolicy: Never
```

If you have created a Persistent Volume and Persistent Volume Claim, mount it as a volume to the workload as in the below example:

```copy
spec:
  volumes:
    - name: task-pv-storage
      persistentVolumeClaim:
        claimName: task-pv-claim
  containers:
    - name: task-pv-container
      image: <IMAGE>
      volumeMounts:
          # Provide the path as per application
        - mountPath: "/usr/data/app"
          name: task-pv-storage
```

For the **Deployment** specify the *replicas* as the number of pods to create and manage.

For the **Cronjob** specify the *schedule* in the Linux crontab format. For example:

```
0 12 * * *
Run at 12 noon everyday
```



###8. Create Service

Create a servcie to expose your application running on a set of pods as a network service.

Service *type* can be NodePort, ClusterIP or LoadBalancer.

*targetPort* is the port the application is running on. It should be the port mentioned in the *containerPort* in the workload.

*port* is the port where you want to expose the service.

You can add more than one ports

```copy
spec:
  type: ClusterIP
  selector:
    app: app-label
  ports:
  - name: app-port1
    protocol: TCP
    port: 4000
    targetPort: 5432
  - name: app-port2
    protocol: TCP
    port: 3000
    targetPort: 8080
    
```





**Save all the yaml files.**
