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

If the application involves saving data in a Storage then create a **PersistentVolumeClaim**

Set the accessModes to one or mroe of - ReadWriteOnce, ReadOnlyMany, ReadWriteMany, ReadWriteOncePod



###7. Create Pods 

Pod

Deployment

Statefulsets

Job

Cronjob



###8. Create Service





Save all the yaml files.
