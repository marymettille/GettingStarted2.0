# Getting Started
Welcome to HiringThing! We are so happy to have you. Please follow these instructions below to get the ATS running on your local machine.

If you are unable to complete any steps outlined in this doc or encounter issues be sure to ask for help in the [`#engineering` channel in Slack](https://hiringthing.slack.com/app_redirect?channel=engineering)

## Environment Notes
HiringThing should be developed while running either macOS, Linux, or Windows under WSL2. When using WSL2, you will primarily follow Linux installation instructions for required tools, and install them inside your WSL environment.

## EditorConfig
Install the [EditorConfig plugin](http://editorconfig.org/#download) for your editor of choice.

## AWS Credentials
Once you have completed your security training, you will be sent an email containing your IAM username and password. Once you have this email:
1. [login to the IAM console](https://console.aws.amazon.com/iam/home#/users).
1. Scroll down to your IAM user
1. Select the "Security Creditials" pane
1. In the "Access Keys" section click the "Create Access Key" button
    1. **This will be the only time you will see the provided password! Make sure to save it in LastPass immediately** 

## AWS CLI
Install the [AWS CLI](https://aws.amazon.com/cli/).

Configure your aws creditials in your terminal. The quick start can be seen below (substitute with your actual credentials):
```
$ aws configure
AWS Access Key ID [None]: $YOUR_DEV_AWS_ACCESS_KEY_ID
AWS Secret Access Key [None]: $YOUR_DEV_AWS_SECRET_ACCESS_KEY
Default region name [None]: us-east-1
Default output format [None]: json
```

Validate your credentials with the following command:
```
$ aws sts get-caller-identity
```

This should output your AWS IAM user's ID, HiringThing's Dev AWS Account `618559405525`, and your AWS IAM user's [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html):
```
{
    "UserId": "$YOUR_DEV_AWS_ACCESS_KEY_ID",
    "Account": "618559405525",
    "Arn": "arn:aws:iam::618559405525:user/$YOUR_IAM_USERNAME"
}
```

[For additonal documentation, please see](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) 

## Install Ruby
Install [Ruby](https://www.ruby-lang.org/en/documentation/installation/) on your host. The exact version doesn't matter, this will only be used for the `docker-sync` utility below.

* Note: If you are installing on a mac, I recommend [using this link here to create a ruby environment for a mac](https://github.com/monfresh/laptop)

## Install Docker
Install **Docker** for:
- [Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
  - **Make sure Docker for Mac allocates at least 4GiB of memory. You can do this by opening Docker, going to settings, then resources, then resetting the memory. 8.00 GB is reccommended**
- [Linux](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)
  - [Docker-Compose](https://docs.docker.com/compose/install/) needs to be installed separately in linux
- [Windows](https://docs.docker.com/docker-for-windows/wsl/)
  - Ensure you follow the [setup instructions for WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10) first.
  **IMPORTANT** It is recommended that Ubuntu 20.04 be the selected distro for this installation

Ensure that docker is working correctly by running the `hello-world` program

```sh
docker run hello-world
```

## Install docker-sync
[`docker-sync`](http://docker-sync.io/) requires ruby to be installed on your host.

```sh
gem install docker-sync
```

## Ensure that git is installed on your local machine.
You can check this by entering ```git --version``` in your terminal.

## Set up your Github SSH keys
Github's documentation for this is pretty thorough. [Please follow these steps](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

Test that you are connected by entering ```ssh _T git@github.com```

## Cloning the repo
- Clone the repo and switch to the 'master' branch
```git clone git@github.com:hiringthing/HiringThing.git```
- Configure your git user for the HiringThing project:

```cd HiringThing/
git config user.name "Your Name"
git config user.email "yournewwmail@hiringthing.com"
git config branch.autosetuprebase always
```
- Ensure that your HiringThing email address is in your Github settings
