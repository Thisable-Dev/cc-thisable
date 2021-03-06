[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FThisable-Dev%2Fcc-thisable%2Ftree%2Fmain%2Fbug-reports-v2&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)


# Bug Reports API Implementation

What is **Bug Report**? A bug report is a specific report that outlines information about what is wrong and needs fixing within Application. The report lists reasons, or seen errors, to point out exactly what is viewed as wrong, and also includes a request and/or details for how to address each issue. [References](https://bugherd.com/blog/bug-reporting/#:~:text=A%20bug%20report%20is%20a,how%20to%20address%20each%20issue.)

How is that **important**? Bug reporting helps smooth out Application, so that it does what it needs to, without frustrating the people using it. Nobody wants to work with software that doesn’t behave as expected. It’s a terrible user experience. [References](https://bugherd.com/blog/bug-reporting/)

In this project, I made Bug Reports API with **RESTful API** method and Authentication API Caller using **Hapi-auth-jwt** and **jsonwebtoken**. Deploy the code to **VM Instance** and store the request body data json to **Firestore Database**

## Table of Contents
<details open>
<summary><b>(click to expand or hide)</b></summary>
<!-- MarkdownTOC -->

1. [Cloud Architecture](#cloud-architecture)
1. [API Endpoints for Android](#api-endpoints)
1. [Code Explanation and References](#code-and-references)
1. [Google Cloud Platform Infrastructure](#gcp-infrastructure)
  
<!-- /MarkdownTOC -->
</details>

# Cloud Architecture
<a id="cloud-architecture"></a>
![Cloud Architecture](https://user-images.githubusercontent.com/57006944/178407865-c30909a3-2559-408f-85c0-bccf3b120efa.jpg)

<a id="api-endpoints"></a>
# API Endpoints for Android
Send Data and Get Data
```
http://34.101.33.206:5000/report?token=<secret-token> [POST]
http://34.101.33.206:5000/report?token=<secret-token> [GET]
```

<a id="code-and-references"></a>
# Code Explanation and References
So, How to authenticate api calling? let's just see into my package.json first
```json
{
  "name": "bug-reports-v2",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start-production": "NODE_ENV=production node ./src/server.js",
    "start-development": "nodemon ./src/server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@google-cloud/firestore": "^5.0.2",
    "@hapi/hapi": "^20.2.2",
    "firebase-admin": "^10.2.0",
    "hapi-auth-jwt2": "^10.2.0",
    "jsonwebtoken": "^8.5.1",
    "nanoid": "^3.3.4"
  },
  "devDependencies": {
    "nodemon": "^2.0.16"
  }
}
```
In code above, the depedencies I used are **Firestore**, **HapiJS**, **Firebase Admin**, **Hapi Auth JWT**, **jsonwebtoken**, and **nanoid**.

## 1. HapiJS
Read the [full documentation](https://hapi.dev/tutorials/?lang=en_US).

Basically, I'm using HapiJS RESTful API to allow HTTP request/response to our server.
### Installation
```sh
npm install @Hapi/hapi
```

## 2. Firebase Admin
Read the [full documentation](https://firebase.google.com/docs/admin/setup).

I put the configuration connection in handler.js
```sh
var admin = require("firebase-admin")
admin.initializeApp({
    credential: admin.credential.cert(serviceAccount)
  })
```

## 3. Connect to Firestore Databse
Read the [full documentation](https://cloud.google.com/firestore/docs/create-database-server-client-library#linux-or-macos).

Connect with firestore database:
```sh
const db = admin.firestore()
```
**How to add POST body request data to Firestore Database?** Add This Code Below (Depends on your code config)
```
 // Payload Data write to Firestore collection "bugreports" and generate new random ID
    db.collection("bugreports").doc(ID_DocumentReport).set(newReport)
    const CheckDatabase = db.collection("bugreports").doc(ID_DocumentReport)
    if (CheckDatabase) {
        const response = h.response({
            status: 'success',
            error: false,
            message: 'Report is successfully added to Firestore',
            date: createdAt,
            data: {
                reportId: ID_DocumentReport
            }
        })
        response.code(201)
        response.header("Authorization", request.headers.authorization)
        response.type('application/json')
        return response
    }    
```

## 4. Hapi Auth JWT
Read the [full documentation](https://www.npmjs.com/package/hapi-auth-jwt2)
I need to auth the API call, hence we create auth strategy for it
```sh
npm i hapi-auth-jwt2
```
### You need to register plugin within HapiJS Framework
```
// Server Register Plugin
await server.register(hapiAuthJWT)
```
### JWT Auth Plugin Strategy inside HapiJS Framework
```
 // JWT Auth Strategy (Param1: Strategy, Param2: Scheme, Param3: scheme requirements)
    server.auth.strategy('jwt', 'jwt', {
        key: secret,
        validate,
        verifyOptions: { ignoreExpiration: true }
    })
    server.auth.default('jwt')
```

## 5. jsonwebtoken
Read the [full documentation](npm i jsonwebtoken)
```sh
npm i jsonwebtoken
```
### Usage
Setup your hapi.js server as described above (no special setup for using JWT tokens in urls)
```sh
https://yoursite.co/path?token=your.jsonwebtoken.here
```
You need to check whether your token is valid signature or not in [jwt.io](https://jwt.io/)

example of Valid Signature:
```sh
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
## 6. NanoID
Read the [full documentation](https://www.npmjs.com/package/nanoid).

NanoID is a tiny, secure, URL-friendly, unique string ID generator for JavaScript.
Installation
```sh
npm i nanoid
```

<a id="gcp-infrastructure"></a>
# Google Cloud Platform Infrastructure
### Installation
Bug Reports API uses [GCE/VM Instances](https://cloud.google.com/compute/docs/instances) and [Firewall](https://cloud.google.com/vpc/docs/firewalls#firewall_rule_components) to run

### 1. Deploy VM Instance
```sh
gcloud compute instances create bugreports-server-vm --project=devthisable --zone=asia-southeast2-b \ 
--machine-type=e2-micro \
--network-interface=network-tier=PREMIUM,subnet=default \
--metadata=^,@^ssh-keys=c2297f2531:ecdsa-sha2-nistp256\ AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBL2j1\+DliXi7BerhfCI4WcMVClWBQAVQepY4j7Vl9j5QR4rT/fUXoIr6q4TI4NkiUWhA2IH9y7QXsNwBkxzTOPA=\ google-ssh\ \{\"userName\":\"c2297f2531@bangkit.academy\",\"expireOn\":\"2022-06-08T11:16:52\+0000\"\}$'\n'c2297f2531:ssh-rsa\ AAAAB3NzaC1yc2EAAAADAQABAAABAQCLeoTU2\+9FZwfLOHMPFoPi1/G9KOB3Lvz8AE5QschheHobXC30WfmEwws3u1ivUaJm9ZwxFb1QkjIrleE55oLXCTv0ZUAtcVpzHfuujWocY7HlrijOaIicz/74gll7Rmy6PLmJApfiVOvCo9J7j1zhDuBSfP8trDXhOAkthNWbYUzlC0DZWLxNh/ik\+Otq3WmExVukKvfDZsU0X\+xxO0EeN2NK1u1DnNVQm3xTUsBmnoUH2FOMaPUNWXdZP54mLwCHjBkPsTxzyWcM32caORGU0c5dD3rrKK8AwTNvjiQ/tRj6r39aJJMDJu7bhTj6NeEvYIhoy87s\+MOg/Q1jYGTJ\ google-ssh\ \{\"userName\":\"c2297f2531@bangkit.academy\",\"expireOn\":\"2022-06-08T11:17:08\+0000\"\} \
--maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=76310350536-compute@developer.gserviceaccount.com 
--scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append \ 
--tags=app-server,http-server,https-server \ 
--create-disk=auto-delete=yes,boot=yes,device-name=bugreports-server-vm,image=projects/ubuntu-os-cloud/global/images/ubuntu-2004-focal-v20220419,mode=rw,size=10,type=projects/devthisable/zones/asia-southeast2-b/diskTypes/pd-balanced \
--no-shielded-secure-boot \
--shielded-vtpm \ 
--shielded-integrity-monitoring \
--reservation-affinity=any
```
---
### 1.1 Extras: Inside bugreports-server-vm instance SSH
#### 1. Set local-time in SSH
- You need to get the password of your OS Disk - for me it's Ubuntu 20 [References](https://stackoverflow.com/questions/70774352/is-there-any-solution-to-gain-access-of-the-password-of-user-account-in-vm-insta)
```sh
$ sudo su
$ sudo passwd
$ password: <type-your-password-here>
```
- Then, configure timedatectl (you will be assigning to auth with your OS disk password)
```sh
$ timedatectl
$ timedatectl set-timezone Asia/Jakarta
```
Output of cli above:
```
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: alice
Password: 
```
Type in your password and you are set!
#### 2. Cloning production repo [Disclaimer: I'm using private repo, so this link below just for example]
```sh
$ git clone https://github.com/Thisable-Dev/cc-thisable.git
```
#### 3. Install NVM [Node Version Manager]
```sh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```
#### 4. Match the local node version with bugreports-server-vm instance
```sh
$ nvm install <NodeJS version>
```
NodeJS version example: v14.17.0
#### 5. Change directory to bugreports folder 
```sh
$ cd bugreports
```
#### 6. Install your application package
```sh
$ npm install
```
#### 7. Run development/production stage
```sh
$ npm run start-production
```
#### 8. Install Process Manager (outside cd bugreports)
```sh
$ npm install -g pm2
```
#### 9. Start Production with Process Manager (inside cd bugreports)
```sh
$ pm2 start npm --name "bugreport-auth-api" -- run "start-production"
```
---
### 2. Create Firewall rule to allow ingress connection to VM
```sh
gcloud compute --project=devthisable firewall-rules create bugreport-fw-allow-access \ 
--description="this firewall is to allow public request to access the bug reports handler in the port 5000" \ 
--direction=INGRESS \
--priority=1000 \ 
--network=default \ 
--action=ALLOW \
--rules=tcp:5000,tcp:22,tcp:3389,tcp:0-65535,icmp \
--source-ranges=0.0.0.0/0 \ 
--target-tags=app-server
```
