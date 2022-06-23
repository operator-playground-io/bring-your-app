###1. Clone the k8s templates

```execute
cd /home/student/code-server/ && git clone https://github.com/operator-playground-io/build-k8s-resources.git
```

###2. Create pre-requisite directory

```execute
mkdir -p /home/student/code-server/build-k8s-resources/$AppName
cd /home/student/code-server/build-k8s-resources/$AppName
```

###3.Use the Developer Dashboard to create k8s resources

![codestructure](_images/IDE.png)

###4. Deploy the application on k8s

```execute
kubectl apply -f *
```