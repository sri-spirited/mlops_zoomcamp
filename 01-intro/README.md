Below are my notes from Datatalk's MLOps zoomcamp. 

## 1.1 Introduction

<a href="https://www.youtube.com/watch?v=s0uaFZSzwfI&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK">
</a>



## 1.2 Environment preparation

### 1.2.1 GitHub Codespaces
<a href="https://www.youtube.com/watch?v=MzcmWXYxi2s&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK&index=2">
</a>


OR ( I skipped AWS!)

### 1.2.2 VM in AWS

<a href="https://www.youtube.com/watch?v=IXSiYkP23zo&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK">
</a>


Code:

Recommended development environment: Linux

### Step 1: Download and install the Anaconda distribution of Python

Download conda installer
```sh
wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
```

Install conda (run the downloaded installer script which installs Anaconda into your home directory)
```sh
bash Anaconda3-2022.05-Linux-x86_64.sh
```

Run `bash` to start a new shell 

Now when you run `python -V` in your shell, instead of showing
`/home/codespace/.python/current/bin/python` 
it will point to `/home/codespace/anaconda3/bin/python`
(The new shell instances sources the updated shell init files which have Anaconda bin directory at the beginning of `PATH`. Therefore, Conda instance of python is used.)
`

Now you can launch your notebook using `jupyter notebook`, and it is running on codespaces. 

### Step 2: Update existing packages

```sh
sudo apt update
```

### Step 3: Install Docker and Docker Compose
Follow the instructions here:
[install-using-the-repository](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)  
Set up Docker's apt repository.

Update the package lists
```sh
sudo apt-get update
```
Install packages `ca-certificates` (for secure communication over the internet) and `curl` (CL tool for transmitting data with URLs, will be used to download Docker's GPG key).

```sh
sudo apt-get install ca-certificates curl
```

Create a directory `etc/apt/keyrings` with permission set to `0755` (readable and executable by everyone, writeable by owner)
```sh
sudo install -m 0755 -d /etc/apt/keyrings
```
Download Docker's official GPG key & save to `etc/apt/keyrings/docker`. The `fsSL` option sets curl to fail silently, show errors and follow redirects. 
```sh
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```
Change permissions of the GPG key to be readable by all users. 
```sh
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

# Add the repository to Apt sources:
```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Install the Docker packages.
```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
To run docker without `sudo`:

```sh
sudo groupadd docker
sudo usermod -aG docker $USER
```

### Step 4: Run Docker

```sh
docker run hello-world
```

If you get `docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.` error, restart your VM instance, or run:
`sudo dockerd`

**Note**: If you get `It is required that your private key files are NOT accessible by others. This private key will be ignored.` error, you should change permits on the downloaded file to protect your private key:

 ```sh
chmod 400 name-of-your-private-key-file.pem
```

## 1.3 (Optional) Training a ride duration prediction model

**Note**: The NYC taxi data is now in parquet format, not CSV.
Here's a [video](https://www.youtube.com/watch?v=r94QjpX9vSE&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK) that explains how to 
read parquet data.

<a href="https://www.youtube.com/watch?v=iRunifGSHFc&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK">
</a>

Links:

* [Notebook](duration-prediction.ipynb)


## 1.4 Course overview

<a href="https://www.youtube.com/watch?v=teP9KWkP6SM&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK">
</a>



## 1.5 MLOps maturity model

<a href="https://www.youtube.com/watch?v=XwTH8BDGzYk&list=PL3MmuxUbc_hIUISrluw_A7wDSmfOhErJK">
</a>

Links: 

* [MLOps Maturity model](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/mlops/mlops-maturity-model)

A summary (from [here](https://github.com/balapriyac/DTC-MLOps-Zoomcamp/blob/main/week1/MLOps-maturity-levels.md?plain=1))

|Level|Description|Overview|When Should You Use?|
|---|---|---|---|
|0️⃣|No Automation 😢|<ul><li>All code in Jupyter Notebook</li><li>No pipeline, experiment tracking, and metadata</li> </ul>|<ul><li>Academic projects</li><li>Proof of Concept is the end goal, not production-ready models</li></ul>|
|1️⃣|Yes! DevOps😀, No MLOps|<ul><li>Best engineering practices followed</li><li>Automated releases</li><li>Unit \& Integration Tests</li><li>CI/CD pipelines</li><li>No experiment tracking and reproducibility</li><li>Good from engineering standpoint, models are not ML-aware yet!</li></ul>|<ul><li>Moving from proof of concept to production</li><li>When you need some automation</li><ul>|
|2️⃣|Automated Training 🛠|<ul><li>Training pipelines</li><li>Experiment tracking</li><li>Model registry (track of currently deployed models)</li><li>Data scientists work in tandem with the engineering team</li><li>Low friction deployment</li></ul>|<ul><li>When you have increasing number of use cases</li><li>Three or more use cases, you should definitely consider automating!</li><ul>|
|3️⃣|Automated Deployment 💬|<ul><li>Model deployment simplified!</li><li>Prep data >> Train model >> Deploy model</li><li>A/B testing</li><li>Model X: v1, v2 >> v2 is deployed; how to ensure v2 performs better?</li><li>Model monitoring</li></ul>|<ul><li>Multiple use cases</li><li>More mature + important use cases</li><ul>|
|4️⃣|Full MLOps Automation ⚙ |<ul><li>Automated training</li><li>Automated retraining</li><li>Automated deployment</li></ul>|<ul><li>Check if level 2, 3 won't suffice</li><li>Should model retraining and deployment be automated as well?</li><li>Super important to take a pragmatic decision! Do you really need level 4?😄</li><ul>|

## 1.6 Homework

More information [here](../cohorts/2024/01-intro/homework.md).


## Notes

Useful references

* [GCP Environment Setup by Piyush](https://github.com/piyush-an/MLOps-ZoomCamp/blob/main/01-Introduction/infrastructure.md)
* [Microsoft Azure Environment Setup by Olaide](https://github.com/josepholaide/MLOps-Practice/blob/main/Week%201/README.md)
* [Environment Preparation using GCP and pyenv by Dani](https://github.com/syahrulhamdani/dtc-mlops/blob/main/week-1-introduction/README.md)
* [Useful links by Zioalex](https://github.com/zioalex/mlops-zoomcamp/blob/main/My_notes_week1.md)
* [Notes by Alvaro Navas](https://github.com/ziritrion/mlopszoomcamp/blob/main/notes/1_intro.md)
  * [Environment setup on GCP (recycled from Data Engineering Zoomcamp)](https://gist.github.com/ziritrion/3214aa570e15ae09bf72c4587cb9d686)
  * [Docker cheatsheet](https://gist.github.com/ziritrion/1842c8a4c4851602a8733bba19ab6050)
  * [Conda cheatsheet](https://gist.github.com/ziritrion/8024025672ea92b8bdeb320d6015aa0d)
* [Notes from first lesson by Neimv](https://gitlab.com/neimv/mlops/-/blob/main/lessons_weeks/notes_1.md)
* [Course preliminaries (jupyterbook) by particle1331](https://particle1331.github.io/ok-transformer/nb/mlops/01-intro.html)
* [Notes by Francisco Delca (environment: local ubuntu + virtualvenv)](https://github.com/FDelca/mlops_datatalks_notes/blob/main/Week1/Week1-LearningNotes.ipynb)
* [What is MLOps? - Non-technical intro by Lorenz](https://github.com/LoHertel/Road-to-MLOps/blob/main/01-primer/README.md)
* [Notes for Week1 by Bhagabat](https://github.com/BPrasad123/MLOps_Zoomcamp/tree/main/Week1)
* [Comprehensive guide to MLOps: Theory & Concepts for Beginners by Nithish Prabhu](https://ntp3105.github.io/Comprehensive-MLOps/Week-1/Introduction%20to%20MLOps.html)


