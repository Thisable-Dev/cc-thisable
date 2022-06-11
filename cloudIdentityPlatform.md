# Thisable - Cloud Identity Platform Documentation

What is **Cloud Identity Platform**? Identity Platform is a customer identity and access management (CIAM) platform that helps organizations add identity and access management functionality to their applications, protect user accounts, and scale with confidence on Google Cloud.

Identity Platform can help protect your app’s users and prevent account takeovers by offering multi-factor authentication (MFA) and integrating with Google’s intelligence for account protection. [References](https://cloud.google.com/identity-platform)

## Table of Content

<details open>
<summary><b>(click to expand or hide)</b></summary>
<!-- MarkdownTOC -->

1. [Enable Cloud Identity Platform](#enable-identity-platform)
1. [Add a Provider (Login Method)](#add-provider)
  
<!-- /MarkdownTOC -->
</details>

<a id="enable-identity-platform"></a>
# Enable Cloud Identity Platform

1. Go to Google Cloud Platform.
2. Navigate to Marketplace from navigation menu.
3. Search **"Cloud Identity Platform"** in the search bar.
4. Select the API and press **Enable Identity Platform**.

![Screen Shot 2022-06-11 at 22 36 57](https://user-images.githubusercontent.com/50565813/173196024-a13c3f0e-d401-49d2-a5c9-f5e1406f8455.png)

<a id="add-provider"></a>
# Add a Provider (Login Method)

1. Go to **Identity Platform** page by accessing it through navigation menu or click [here](https://console.cloud.google.com/customer-identity/).
2. Go to **Providers** menu and click **ADD A PROVIDER**.
![image](https://user-images.githubusercontent.com/50565813/173207813-93f5ce4b-2a03-4aa2-9537-5274bdeefa3d.png)
3 Add the provider you want by choosing from the dropdown.
![image](https://user-images.githubusercontent.com/50565813/173207461-cde7455f-fd4d-402b-8d58-8640d0fcf7c4.png)
4 For Google SSO, you have to configure **Web Client ID** and **Web Client Secret** that can be creating through [APIs and Services page](https://console.cloud.google.com/apis/credentials). <br>
5 Press **save** button.

## Connect to Android application

In order to connect it to Android application, we have to use **Firebase** as the backend services for Android app. Usually, the Firebase project is automatically created if we already create Identity Platform through Google Cloud Platform. 

1. Go to [Firebase console](https://console.firebase.google.com)
2. Choose your project ID.
![image](https://user-images.githubusercontent.com/50565813/173207672-4bf58441-2bb3-4a3c-bc3e-4846ffc6a090.png)
3. To add application integration, press **Add app** button. Beforehand, you must input **Android package name**, **SHA-1 Fingerprint certificate**, and **application name**.<br>
![image](https://user-images.githubusercontent.com/50565813/173207682-cd7f1f60-19c4-44de-9b08-5ca32af8afc1.png)
4. After the process finished, it will generated **google-services.json** automatically that will be deployed by Mobile Development later.

**Remember**, that the file contain credentials informations, so keep it at the very safe place.










