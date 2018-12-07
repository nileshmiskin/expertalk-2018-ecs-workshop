# expertalk-2018-ecs-workshop
Repository for ecs workshop for expert talk india conference 2018.

## Pre workshop setup step

#### You need following things installed on your machine

- Git
- Git bash (for windows only)
- Github account (with SSH key configured)
- Virtualbox
(Vagrant can support VirtualBox version 4.0.x, 4.1.x, 4.2.x, 4.3.x, 5.0.x, 5.1.x, and 5.2.x. Other versions are unsupported and you will get an error message. Please note that beta and pre-release versions of VirtualBox are not supported and may not be well-behaved.)
Install with the help of the official installer. https://www.virtualbox.org/wiki/Downloads

- Vagrant (2.2.2)
Do not use a package manager for installing Vagrant. Please use the official installer. https://www.vagrantup.com/downloads.html

- IntelliJ Idea or Eclipse (or any other IDE you are comfortable with)

#### Pull the vagrant box in an empty directory with the following command

Note: Run all commands on Terminal or GitBash (not GitCMD)

```bash
mkdir ecsworkshop
cd ecsworkshop
mkdir basevm
cd basevm
vagrant init prashantkalkar/ecsworkshopbox --box-version 1.0.1
vagrant up
```

#### Test the box is working

Get into vagrant vm.
```bash
vagrant ssh
```
Check docker is working

```bash
vagrant ➜  ~ docker info
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 2
Server Version: 18.06.1-ce
...
```
Check images
```bash
vagrant ➜  ~ docker images
REPOSITORY                                  TAG                 IMAGE ID            CREATED             SIZE
kaushikchandrashekar/ecs-workshop-jenkins   v1.0                2a93fab2fcdc        9 days ago          1.6GB
jenkins/jenkins                             lts                 d7c5abfe8477        4 weeks ago         703MB
```
Check terraform version
```bash
vagrant ➜  ~ terraform -version
Terraform v0.11.10
```
Check java setup
```bash
vagrant ➜  ~ echo $JAVA_HOME
/usr/lib/jvm/java-8-openjdk-amd64/
```
check whether dos2unix is installed

```bash
vagrant ➜  ~ which dos2unix
/usr/bin/dos2unix
```

## Setup steps during workshop

### Fork ecsworkshop repositories

Fork following repositories to your github user

https://github.com/ecsworkshop2018/expertalk-2018-ecs-workshop
https://github.com/ecsworkshop2018/seed_job
https://github.com/ecsworkshop2018/odin

### Clone repositories

Open terminal or git bash (*on host machine not VM*)

```bash
➜  cd ecsworkshop/
➜  git clone git@github.com:prashant-ee/expertalk-2018-ecs-workshop.git
Cloning into 'expertalk-2018-ecs-workshop'...
```

### Download aws access key details (required to configure AWS cli later).

Login to AWS account. Reset password for first time. 

Create a new AWS accessKey and download *accessKeys.csv* to your user home directory. 

### Create GitHub Personal access token (required to create github webhooks on jenkins).

Use following link to setup a personal GitHub access token.

https://support.cloudbees.com/hc/en-us/articles/234710368-GitHub-User-Scopes-and-Organization-Permission

Note it down somewhere as it will not be available again. This is required in the setup below. 

### Setup workspace config

Go to workspace config template and copy it to user home directory.

```bash
➜  cd ecsworkshop/
➜  cd expertalk-2018-ecs-workshop/
➜  cd docker_dev_vagrant/
➜  cp workspace_config.template ~/workspace_config
```
Open ~/workspace_config in any text editor of your choise. Add required information and save the file.

```
FIRST_NAME="<your first name>"
JENKINS_USER_NAME="<your jenkins user name>"
JENKINS_PASSWORD="<your jenkins password>"
GITHUB_USER_NAME="<your github user name>"
GITHUB_USER_EMAIL="<your github user email>"
GITHUB_ACCESS_TOKEN="<your github personal access token>"
SEED_JOB_REPO_URL="<your seed job repo url>"
```
Details:
FIRST_NAME - make sure this is unique, prefer to put the AWS username for this. 
JENKINS_USER_NAME - Choose a name of your Jenkins admin (every participant will have his own Jenkins). 
JENKINS_PASSWORD - Choose a password for your Jenkins admin user.  
GITHUB_USER_NAME - Put in your github user name (git information is used to configure git during workshop)
GITHUB_USER_EMAIL - Put in your github email.
GITHUB_ACCESS_TOKEN - Put in the personal access token we created earlier.
SEED_JOB_REPO_URL - Put in the https github url of the seed job (the one we forked earlier). 

### Build Development box (VM) which are going to use during workshop

```bash
➜  cd ecsworkshop/
➜  cd expertalk-2018-ecs-workshop/
➜  cd docker_dev_vagrant/
➜  vagrant up
➜  vagrant ssh
```

## Setup your jenkins

### Build your jenkins image

```console
vagrant ➜  cd repos/expertalk-2018-ecs-workshop/jenkins_docker/
vagrant ➜  JENKINS_TAG="${FIRST_NAME}-$(date +%s)"
vagrant ➜  cp ~/.ssh/id_rsa ./github_ssh_private_key
vagrant ➜  docker build \
    --build-arg "ROOT_URL=https://${FIRST_NAME}-jenkins.ecsworkshop2018.online" \
    --build-arg "JENKINS_USER_NAME=${JENKINS_USER_NAME}" \
    --build-arg "JENKINS_PASSWORD=${JENKINS_PASSWORD}" \
    --build-arg "GITHUB_USER_NAME=${GITHUB_USER_NAME}" \
    --build-arg "GITHUB_USER_EMAIL=${GITHUB_USER_EMAIL}" \
    --build-arg "GITHUB_ACCESS_TOKEN=${GITHUB_ACCESS_TOKEN}" \
    --build-arg "SEED_JOB_REPO_URL=${SEED_JOB_REPO_URL}" \
    -t ${JENKINS_ECR_REPOSITORY_PATH}:${JENKINS_TAG} .
    ...
vagrant ➜ rm ./github_ssh_private_key
```

### Push jenkins image to ECR in AWS account

```bash
```

## Pre-workshop tasks (for instructor)

### Create AWS users for workshop candidates

### Create state bucket

### Create VPC

```bash

```
