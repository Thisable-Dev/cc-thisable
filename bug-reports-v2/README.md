[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FThisable-Dev%2Fcc-thisable%2Ftree%2Fmain%2Fbug-reports-v2&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)


# Bug Reports API Implementation

What is **Bug Report**? A bug report is a specific report that outlines information about what is wrong and needs fixing within Application. The report lists reasons, or seen errors, to point out exactly what is viewed as wrong, and also includes a request and/or details for how to address each issue. [References](https://bugherd.com/blog/bug-reporting/#:~:text=A%20bug%20report%20is%20a,how%20to%20address%20each%20issue.)

How is that **important**? Bug reporting helps smooth out Application, so that it does what it needs to, without frustrating the people using it. Nobody wants to work with software that doesn’t behave as expected. It’s a terrible user experience. [References](https://bugherd.com/blog/bug-reporting/)


## Table of Contents
<details open>
<summary><b>(click to expand or hide)</b></summary>
<!-- MarkdownTOC -->

1. [Code Explanation and References](#code-and-references)
1. [Google Cloud Platform Infrastructure](#gcp-infrastructure)
  
<!-- /MarkdownTOC -->
</details>

<a id="code-and-references"></a>
## Code Explanation and References
  
<a id="gcp-infrastructure"></a>
## Google Cloud Platform Infrastructure
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
git clone https://github.com/Thisable-Dev/cc-thisable.git
```
#### 3. Install NVM [Node Version Manager]
```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
```
#### 4. Match the local node version with bugreports-server-vm instance
```sh
nvm install <NodeJS version>
```
NodeJS version example: v14.17.0
#### 5. Change directory to bugreports folder 
```sh
cd bugreports
```
#### 6. Install your application package
```sh
npm install
```
#### 7. Run development/production stage
```sh
npm run start-production
```
#### 8. Install Process Manager (outside cd bugreports)
```sh
npm install -g pm2
```
#### 9. Start Production with Process Manager (inside cd bugreports)
```sh
pm2 start npm --name "bugreport-auth-api" -- run "start-production"
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
