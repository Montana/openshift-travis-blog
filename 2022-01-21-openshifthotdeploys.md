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


In this weeks tech blog, I'll show you how you can run Windows and Windows Server  specifically version 1809 with Travis CI. Let's get this started up.

<!-- more --> 

## Getting started 

Let's create a text file called `constraints.txt` and the contents of this file should look like this:

```bash
testinfras==3.3.0
pytest==4.6.8
codecov==2.0.15
```

Now let's create another file called `requirements.txt`, what we need to do is copy the `constraints.txt` file in `requirements.txt, and how we do that is, using the `-c` flag in our `requirements.txt` file. So let's see how our `requirements.txt` file is going to look like:

```bash
-c constraints.txt
testinfra
pytest
codecov
```

## Python 3.8 on Windows Build

So let's start running a build using Windows and Python 3.8 as our default language, let's see this portion of how the `.travis.yml` file will look:

```yaml
fleet_script_tasks:
  script: &ref_1
    - python --version
fleet_install_tasks:
  install: &ref_0
    - pip install -r requirements.txt
matrix:
  fast_finish: true
  include:
    - name: Python 3.8 on Windows
      os: windows
      language: shell
      env:
        - 'PATH=/c/Python38:/c/Python38/Scripts:$PATH'
      before_install:
        - choco install python --version 3.8.1
        - pip install virtualenv
        - virtualenv $HOME/venv
        - source $HOME/venv/Scripts/activate
      install: *ref_0
      script: *ref_1
      after_success:
        - deactivate
```

You'll see in the `before_install:` I used `choco` to install Python, set the virtual environment, and this `.travis.yml` will build, but what if you want to also run Windows Server in parallel? Well, I'll show you how to do that too.

## Windows Server/Python 3.8 

Let's now combine these ideas, so here's my current `.travis.yml`: 

```yaml
fleet_script_tasks:
  script: &ref_1
    - python --version
fleet_install_tasks:
  install: &ref_0
    - pip install -r requirements.txt
matrix:
  fast_finish: true
  include:
    - name: Python 3.8 on Windows
      os: windows
      language: shell
      env:
        - 'PATH=/c/Python38:/c/Python38/Scripts:$PATH'
      before_install:
        - choco install python --version 3.8.1
        - pip install virtualenv
        - virtualenv $HOME/venv
        - source $HOME/venv/Scripts/activate
      install: *ref_0
      script: *ref_1
      after_success:
        - deactivate
    - name: 'Windows Server, version 1809'
      os: windows
      language: shell
      env: 'PATH=/c/Python37:/c/Python37/Scripts:$PATH'
      before_install:
        - choco install python --version 3.7.3
        - python -m pip install virtualenv
        - virtualenv $HOME/venv
        - source $HOME/venv/Scripts/activate
      script:
        - systeminfo
        - wmic OS get OSArchitecture
        - wmic process list full
        - tasklist
        - net start
        - sc query
      after_success:
        - deactivate
 ```
 
 At this point in the build, you've now added Windows Server, version 1809. I've also added some verbose commands to be ran in the `.travis.yml` like `netstat`, and `sc query`. Now try running the build, and watch it succeed! 
 
 
## Conclusion 

There you go, you just used Windows, Windows Server and used some tools like `choco` and used verbose commands to get a broader scale of your build. 

As always if you have any questions, any questions at all, please email me at [montana@travis-ci.org](mailto:montana@travis-ci.org).

Happy building!
 
