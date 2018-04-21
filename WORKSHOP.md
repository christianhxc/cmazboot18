#Prerequisites
- Docker for Windows (Mac) Edge
- VS2017
- .NET Core 2.X
- kubectl

```
kubectl config unset users.clusterUser_cmazboot18_cmazboot18
```
```
kubectl config unset contexts.cmazboot18
```
```
kubectl config unset clusters.cmazboot18
```

```
docker stop $(docker ps -a -q)
```
```
docker rm $(docker ps -a -q)
```

#Create Cluster
```
az group create --name cmazboot18 --location eastus
```
```
az aks create --resource-group cmazboot18 --name cmazboot18 --node-count 1 --generate-ssh-keys
```
```
az aks get-credentials --resource-group=cmazboot18 --name=cmazboot18
```
```
az aks browse --resource-group cmazboot18 --name cmazboot18
```

#Create Repo with Docker Support
```
dotnet new razor
```
- Open VS and "Add Docker Support"

#Run locally
- Use VS to run container and debug
- # COPY *.sln ./
- # WORKDIR /src/
- COPY azboot18.csproj ./
- docker-compose.dcproj >> .dockerignore

#Deploy locally with k8s
```
kubectl config use-context docker-for-desktop
```
```
kubectl get pods
```
```
kubectl proxy
```
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```

http://localhost:8001/ui

#Deploy locally
```
docker build -t christianhxc/azboot18:1.0 .
```
```
docker tag azboot18:dev christianhxc/azboot18:1.0
```
```
kubectl apply -f webapp-deployment.yaml
```

--Make sure IIS is not running locally
http://localhost/ 

#Create Git repo and push
```
git init
git add .
git commit -m "Genesis"
git remote add origin https://github.com/christianhxc/cmazboot18.git
git push -u origin master
```

#Create Project in VSTS
```
git remote rm vsts
git remote add vsts https://cmelendeztech.visualstudio.com/_git/azbootcamp18
git push vsts master
```

https://github.com/OSSCanada/vsts_build_pipeline/blob/master/hol-content/02-build_vsts_ci.md

- https://almvm.azurewebsites.net/labs/vstsextend/kubernetes/#exercise-1-service-endpoint-creation
- Add service point
- Download the credentials again!!

https://github.com/OSSCanada/vsts_build_pipeline/blob/master/hol-content/03-build_vsts_cd.md

#Clean Up
```
az aks delete --resource-group cmazboot18 --name cmazboot18
az group delete -n cmazboot18
```