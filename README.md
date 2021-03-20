# Full Stack Development
Diary of full stack software development

# Introduction
There is need to document the entire process of software devlopment with practical steps which are easy to follow for others to learn from. This documentation provides very specific steps on projects management, identification of requirements, software development life cycle, tools and deployment all best on enterprise and best industry practices. The first version of this document pays particular attention to software development - tools, langauges deployment thoughts in form of my go to manual of documented steps on how to achieve particular objectives or go around a nuggy problem in software development and design. ALso some decison choices over convention in some cases.

# Project Management

# Full Stack
I use full stuck meaning from deisgn and developemnt - front end (View), middledware (Control), and backend (Model/Data) using microservices to decouple these design concerns to deployment considering aspects of containerising and software orchestration (automation of depoyment/management) using docker and kubernetes. Further this is looks at continous improving of software and applying the patches with minimals disruption of the system.
### Design and development Best Practices

# Tools and Languages

# Front End - View

In designing the client, Ia am looking at having a client flexiibility to be ugraded an improved on without looking too much about the middle ware and the data. The tools I have decided to use is flutter. I can build mobile, web and desktop applications from the same code base.

## Installation of flutter

## Creating a project

```console
~ flutter create [folder_name]
```
**name should have underscore according to flutter**

```console
~ mkdir [folder_name] && cd [folder_name] && flutter create .
```
## Running flutter application
You can check what devices are available and running first:
```console
~ flutter devices
2 connected devices:

Android SDK built for x86 (mobile) • emulator-5554 • android-x86    • Android 10 (API 29) (emulator)
Chrome (web)                       • chrome        • web-javascript • Google Chrome 89.0.4389.90
```
Choosing the device then run in the root of application/project
```console
flutter run -d chrome
```
clean the flutter project due to unknwon errors 
```console
~ flutter clean
```
after changes pubspec.yaml file or you do a clean like in above run the update dependencies command to fetch them where ever they may be sitting
```console
~ flutter pub get
```
## How to use the runnable command to generate "part" files especially when using the json_serializable module
flutter pub run build_runner build
## Null Safety - Dart
sometimes if there is no compatible code add the switch
```console
--no-sound-null-safety
```
# Middleware - Controller
I chose java and spring-boot to build the middleware. 
# Data - Model

# Overal architectural design and decision

# Micros services

# Seperation of business concern

# Authentication

# Versioning

# Git/GitHut

# Deployment and Maintenance
## Deplyment
Docker and kubernetes are for packaging, automating and managing the completed system.
## Java Deployment
After design and testing a microservice it can be deployment to its happily thereafter home in the cloud. An image needs to be created for it and uploaded to the [docker hub]http://hub.docker.com an account is required.

# Docker
# Why docker
## Installation
## Common commands
## Deploying Java to production
1. Packing
2. Pushing to hub dot docker
3. pulling to production environment

# Kubernetes - microk8s
## Why kubernetes
## Installation
## Kubernetes Dashboard
## Running a deplyment
## Expose a service
## Creating a proxy to access using Nginx
## Common Kube commands


