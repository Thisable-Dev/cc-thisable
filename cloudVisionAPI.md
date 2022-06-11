# Thisable - Cloud Vision API Documentation

What is **Cloud Vision API**? The Google Cloud Vision API allows developers to easily integrate vision detection features within applications, including image labeling, face and landmark detection, optical character recognition (OCR), and tagging of explicit content. [References](https://codelabs.developers.google.com/codelabs/cloud-vision-api-python#:~:text=The%20Google%20Cloud%20Vision%20API,the%20Vision%20API%20with%20Python.)

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
4. Select the API and press **Enable Vision API**.


<div align="center">
  <img width="529" alt="Screen Shot 2022-06-11 at 23 08 28" src="https://user-images.githubusercontent.com/50565813/173195714-d26b9d8f-e573-4353-966a-9564563be56a.png">
  <br>
  <i>Cloud Vision API on GCP marketplace</i>
</div>

<a id="integrate-vision-api"></a>
# Integrate with Android Application

There are 2 ways to use the Cloud Vision API, it can be processed through Cloud Functions or you can request via the provided API endpoint. In this case study, we send a POST request to an endpoint that has been provided by Google Cloud. Here are the targeted endpoints.

```
https://vision.googleapis.com/v1/images:annotate
```

To be able to access the endpoint, an API key is needed that can be generated via the [credentials page](https://console.cloud.google.com/apis/credentials?project=_&authuser=0) on the Google Cloud Platform. After all the required information is available, then it will be processed by the Mobile Development team.
