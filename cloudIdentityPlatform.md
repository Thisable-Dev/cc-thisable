# Thisable - Cloud Identity Platform Documentation

What is **Cloud Identity Platform**? Identity Platform is a customer identity and access management (CIAM) platform that helps organizations add identity and access management functionality to their applications, protect user accounts, and scale with confidence on Google Cloud.

Identity Platform can help protect your app’s users and prevent account takeovers by offering multi-factor authentication (MFA) and integrating with Google’s intelligence for account protection. [References](https://cloud.google.com/identity-platform)

## Table of Content

<details open>
<summary><b>(click to expand or hide)</b></summary>
<!-- MarkdownTOC -->

1. [Enable Cloud Vision API](#enable-vision-api)
1. [Integrate with Android Application](#integrate-vision-api)
  
<!-- /MarkdownTOC -->
</details>

<a id="enable-vision-api"></a>
# Enable Cloud Vision API

1. Go to Google Cloud Platform.
2. Navigate to Marketplace from navigation menu.
3. Search **"Cloud Vision API"** in the search bar.
4. Select the API and press **Enable Identity Platform**.
![Screen Shot 2022-06-11 at 22 36 57](https://user-images.githubusercontent.com/50565813/173195089-03361902-5b51-434c-8104-3bc44cfef0cd.png)
<div align="center">Cloud Identity Platform on GCP marketplace.</div>

<a id="integrate-vision-api"></a>
# Integrate with Android Application

There are 2 ways to use the Cloud Vision API, it can be processed through Cloud Functions or you can request via the provided API endpoint. In this case study, we send a POST request to an endpoint that has been provided by Google Cloud. Here are the targeted endpoints.

```
https://vision.googleapis.com/v1/images:annotate
```

To be able to access the endpoint, an API key is needed that can be generated via the [credentials page](https://console.cloud.google.com/apis/credentials?project=_&authuser=0) on the Google Cloud Platform. After all the required information is available, then it will be processed by the Mobile Development team.

