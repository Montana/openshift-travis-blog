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

Hello again, it's Friday! So let's talk tech. We are going to be discussing OpenShift, in particular "Hot deployments." Hot Deployment is the process of adding new components (such as WAR files, EJB Jar files, enterprise Java beans, servlets, and JSP files, let's see what I can teach you, and maybe help you with on your future build. Let's turn up the heat, and read about OpenShift Hot Deploys. 

<!-- more --> 

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

## Conclusion 

There you go, you just used Windows, Windows Server and used some tools like `choco` and used verbose commands to get a broader scale of your build. 

As always if you have any questions, any questions at all, please email me at [montana@travis-ci.org](mailto:montana@travis-ci.org).

Happy building!
 
