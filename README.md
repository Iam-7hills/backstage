# Backstage Installation 

Please follow this steps to install backstage in your Kubernetes cluster

<br>

## Pre-requisite

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

### Create your own backstage application
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

### How to install backstage in Kubernetes cluster
1. cd to application directory
```
cd **FirstBackstage**/
```
```
yarn tsc
yarn build:backend
```
Dockerfile will get created under <FirstBackStage/packages/backend/backend
