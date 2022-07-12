[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FThisable-Dev%2Fcc-thisable%2Ftree%2Fmain%2Fbug-reports-v2&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)


# Bug Reports Deployment

What is **Bug Report**? A bug report is a specific report that outlines information about what is wrong and needs fixing within Application. The report lists reasons, or seen errors, to point out exactly what is viewed as wrong, and also includes a request and/or details for how to address each issue. [References](https://bugherd.com/blog/bug-reporting/#:~:text=A%20bug%20report%20is%20a,how%20to%20address%20each%20issue.)

How is that **important**? Bug reporting helps smooth out Application, so that it does what it needs to, without frustrating the people using it. Nobody wants to work with software that doesn’t behave as expected. It’s a terrible user experience. [References](https://bugherd.com/blog/bug-reporting/)

In this project, I made Bug Reports Deployment with **App Engine**, **RESTful API** HTTP method. Deploy the code to **App Engine** and get the request body data json from **Firestore Database**

## Table of Contents
<details open>
<summary><b>(click to expand or hide)</b></summary>
<!-- MarkdownTOC -->

1. [Cloud Architecture](#cloud-architecture)
1. [Code Explanation and References](#code-and-references)
1. [Google Cloud Platform Infrastructure](#gcp-infrastructure)
  
<!-- /MarkdownTOC -->
</details>

<a id="cloud-architecture"></a>
# Cloud Architecture
![Cloud Architecture](https://user-images.githubusercontent.com/57006944/178407865-c30909a3-2559-408f-85c0-bccf3b120efa.jpg)

<a id="gcp-infrastructure"></a>
# Google Cloud Platform Infrastructure
Bug Reports Deployment uses App Engine

### 1. Make app.yaml manifest file
```
runtime: nodejs
env: flex

service: bugreports-website
```
### 2. Auth/login to Google SDK
```
gcloud auth login
```
### 3. Run "gcloud app deploy"
```
gcloud app deploy
```
### 4. Run "gcloud app browse"
```
gcloud app browse
```
