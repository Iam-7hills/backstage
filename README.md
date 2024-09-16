# Backstage Installation in Kubernetes Cluster

Please follow this steps to install backstage in your Kubernetes cluster. Below steps also located in backstage.io under various location.

<br>

## Pre-requisite

### Login into Kubernetes cluster 

1. NodeJs Installation
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
2. Install Yarn module from npm Note : npm install yarn
```
nvm install 20
nvm --version
```
3. Install Yarn
```
npm install --global yarn
```
4. Set Yarn to major version 1, DO NOT TRY version 2 or any
```
yarn set version 1.22.19
```
5. Verify the Yarn version
```
yarn --version
```
5. Download the git code from backstage
```
git clone git@github.com:backstage/backstage.git
```
<br>
<br>

## Verify the downloaded code if working or not
### Create your own backstage application and verify
1. cd to directory where you have app-config.yaml, package.json 
```
npx @backstage/create-app@latest
```
2. Enter your application name(any): FirstBackstage
cd to your application directory (cd FirstBackstage)
```
cd **FirstBackstage**/
```
3. Run backstage in a development mode
```
yarn dev
```
4. Now you can access the application from the browser
```
http://localhost:3000
```
<br>
<br>

## Postgres and Backstage container deployment
### Actual Installation steps
1. cd to application directory
```
cd **FirstBackstage**/
```
```
yarn tsc
yarn build:backend
```
Dockerfile will get created under <FirstBackStage/packages/backend/backend

2. Edit Dockerfile
to set environment as development or production, in my example, i have used as "development"
search for "NODE_ENV"
```
ENV NODE_ENV development
yarn install --frozen-lockfile --development --network-timeout 300000
```
# Need two containers postgres doesn't require any customization, however you need to perform step 3 and 5 for every changes in the backstage app or the plugins

3. Create Docker Image for the backend container
```
docker login
docker image build . -f packages/backend/Dockerfile --tag iam7hills/dockerdemo:2.0.0
docker push <your-repository>:<tag>
```
please note your repository location, as should be updated under the image section of this file https://github.com/Iam-7hills/backstage/blob/main/backstage/bs-deploy.yaml

4. Run postgres inside kubernetes backstage namespace
```
git clone https://github.com/Iam-7hills/backstage.git
cd backstage/postgres
kubectl create ns backstage
kubectl create -f .
```

5. Run backstage inside kubernetes backstage namespace
```
git clone https://github.com/Iam-7hills/backstage.git
cd backstage/backstage
kubectl create -f .
```

6. Verify the postgres containers if it is running
```
kubectl get all -n backstage
NAME                             READY   STATUS    RESTARTS        AGE
pod/backstage-555c7fb84d-dzck6   1/1     Running   0               42m
pod/postgres-86d6584975-dvxdg    1/1     Running   1 (3h39m ago)   7h20m

NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/backstage   ClusterIP   <ip>    <none>        80/TCP     42m
service/postgres    ClusterIP   <ip>   <none>        5432/TCP   7h20m

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/backstage   1/1     1            1           42m
deployment.apps/postgres    1/1     1            1           7h20m

NAME                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/backstage-555c7fb84d   1         1         1       42m
replicaset.apps/postgres-86d6584975    1         1         1       7h20m
```

