###1. Application name

Enter a name without whitespaces of the application.

```copy
export AppName=<REPLACE APP NAME>
```

###2. Create the namespace

Create the namespace where the application with be created.
```execute
kubectl create namespace $AppName
```