---
title: "OpenShift Hot Deploys on Travis CI"
created_at: Fri Jan 21 2022 15:00:00 EDT
author: Montana Mendy
layout: post
permalink: 2022-01-21-openshifthotdeploys
category: news
excerpt_separator: <!-- more --> 
tags:
  - news
  - feature
  - infrastructure
  - community
---


![TCI-OpenShift](https://user-images.githubusercontent.com/20936398/150583448-e2596870-8f44-4d02-81d5-a4e17bb0dd01.png)

We are going to be discussing OpenShift, in particular "Hot deployments."  Hot Deployment is the process of adding new components (such as WAR files, EJB Jar files, enterprise Java beans, servlets, and JSP files, let's see what I can teach you, and maybe help you with on your future build. Let's turn up the heat, and read about OpenShift Hot Deploys. 

<!-- more --> 

## What is OpenShift 

OpenShift is an abstraction layer on top of Kubernetes (k8s) and provides a useful user interface that in all honesty, `k8s` sometimes lacks.

![image](https://user-images.githubusercontent.com/20936398/150585092-43b53bfa-0c05-4732-a3f1-0067cbf22e61.png)

For reference, look at this simple hierarchical graph to get a better grasp on what OpenShift does.

## OpenShift & Hot Deploys

Testing an OpenShift `hot_deploy` on Travis CI is straight forward. Let's make sure your repository contains `.openshift/markers/hot_deploy`, when you perform a `git push`, OpenShift will not rebuild the application and perform a hot deployment instead.

## Enable Hot Deployments on OpenShift

Open the CLI, and navigate to the directory where you want to create the application. To create a new, let's say `MontanasHotDeployment` application, execute the following command. If you already have an OpenShift Java application, then you can work with that as well. Have a look at the following command:

```bash
rhc create-app myapp montanashotdeployment
```

To enable hot deployment, create a new file with the name hot_deploy inside the .openshift/markers directory. On nix machines, you can create a new file using the touch command as shown in the following command. On Windows machines, you can use file explorer to create a new file. Have a look at the following code:

```bash
touch .openshift/markers/hot_deploy 
```

You'll then want to add the new file to the Git repository index, commit it to the local repository, and then push the changes to the application's remote Git repository:

```bash
git add .openshift/markers/hot_deploy $ git commit –am "enabled hot_deploy"
```

## Deployment hierarchy and Travis CI 

The presence of the `hot_deploy` marker file informs OpenShift that you want to do hot deployment. Before stopping and starting the application cartridges, OpenShift checks for the existence of the`hot_deploy` marker file. Here's a simple `.travis.yml` file I created to have Travis CI call OpenShift: 

```yaml
---
language: java

provider: openshift
  
install: skip
script: echo "hot_deploy test" 
```
It's important to remember in OpenShift there's a GUI that lays on top of `k8s`. 

![image](https://user-images.githubusercontent.com/20936398/150584837-133afbf6-0b8e-4f8f-8630-a5f4dca489fe.png)

The image above is the OpenShift GUI, with various options you can use, and use well with Travis CI. 

## Conclusion 

There you go, you just learned about OpenShift Hot Deploys! So go out there, and deploy, hot or cold -- we look forward to seeing you build with us.

As always if you have any questions, any questions at all, please email me at [montana@travis-ci.org](mailto:montana@travis-ci.org).

Happy building!
