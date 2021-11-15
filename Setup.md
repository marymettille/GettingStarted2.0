# Getting Started
Welcome to HiringThing! We are so happy to have you. Please follow these instructions below to get the ATS running on your local machine .

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
- Ensure that your HiringThing email address is in your Github settings.

If using WSL, ensure the repository lives inside your linux home directory, not on the Windows filesystem.

## Set up local DNS and Certificates
Choose your method of routing requests. The rails server will start up on port `3000` within a container, but Docker will map that to port `80` on the host.

* [Hosts file](dns/hosts.md)
* [DNSMasq on macOS](dns/macos.md)
* [DNSAgent on Windows](https://github.com/stackia/DNSAgent) (Hovis has this working and will be happy to help you)

The dev env uses SSL, and requires adding the dev cert to your host system so your browser will allow access. Follow the instructions here https://hiringthing.atlassian.net/wiki/spaces/HTR/pages/2004320257/Installing+HiringThing+development+root+Certificate+Authority

**Important** Install this certificate on the host machine where you will be using your browser. If you're developing with WSL, install this under Windows.

If you are on Windows and using Firefox, you'll need to follow this process: https://docs.vmware.com/en/VMware-Adapter-for-SAP-Landscape-Management/2.0/Installation-and-Administration-Guide-for-VLA-Administrators/GUID-0CED691F-79D3-43A4-B90D-CD97650C13A0.html

**Note for Junior Engineers** This step involves editing your bash profile. This can be fairly confusing - please send out a message and one of the more senior engineers will be able to walk you through this step.

## AWS ECR Authentication
HiringThing's Docker images are stored in a private Amazon Elastic Container Registry (ECR).
* Install [Amazon's ECR Docker Credential Helper](https://github.com/awslabs/amazon-ecr-credential-helper).
* Edit your `.docker/config.json` file and add the `credHelpers` key.
  * You can do this by entering ```sudo nano ~/.docker/config.json``` in your terminal and entering the below code

  ```
    {
      "credHelpers" : {
        "public.ecr.aws": "ecr-login",
        "618559405525.dkr.ecr.us-east-1.amazonaws.com" : "ecr-login"
      }
    }
  ```
**Note for Junior Engineers** This step can be fairly confusing - please send out a message and one of the more senior engineers will be able to walk you through this step.

# The fun stuff!

### Initial Provisioning
After performing all above steps, from the root directory of the HiringThing ATS git repository run:

```sh
yarn install
```
Then
```sh
./atsdev build # -b || --build
```

This will perform the following on your local machine:
- set the needed `config/database.yml` file
- set the needed `config/application.yml` file
- remove any previously found ATS application Docker resources
- pull any needed base Docker images from Docker Hub and ECR
- start docker-sync
- start the ATS application with Docker Compose

When the `build` completes you will be left with ATS application running locally.

### Rebuilding/Re-Provisioning
Sometimes you may need to rebuild the local dev environment due to upstream updates or changes such as:

- updated base Docker images in use
- updated ruby version
- updated gems/libs

In these cases it may be desirable to rebuild your local dev environment but retain your previous MySQL database to avoid having to recreate/seed it.

From the root dir of the checked out HiringThing ATS git repo run:

```sh
./atsdev rebuild # -r || --rebuild
```

This will perform all steps of a normal `atsdev build` but ensure that the `hiringthing-db` data volume is kept.

## Controlling the ATS Docker Compose Stack
Use the local wrapper script [`./atsdev`](../atsdev) to control the ATS application via Docker Compose for local development.

### Starting
After an initial build has been completed previously if you want to resume using or start the ATS application, from the root dir of the checked out HiringThing ATS git repo run:

```sh
./atsdev start # || up
```

This will perform the following on your local machine:
- start docker-sync
- start the ATS application with Docker Compose
- The Docker compose stack will always run in the foreground (not detached).

### Stopping 
Use `ctrl + c` to exit the stack and stop all containers and docker-sync.

## Make your development company
You can visit [register.applicant-tracking.test](http://register.applicant-tracking.test) to create a new company.

**or**

run `docker-compose run web bin/rails db:seed`. (this gets done for you automatically when building the entire stack from scratch via `./atsdev build`)

This command will populate development database with demo data, create new company ([asdf.applicant-tracking.test](http://asdf.applicant-tracking.test)) and user (*email:* `test@example.com`, *password:* `secret123`).

Populating of the whole database will take about 20 minutes to complete, but you can start to use HiringThing in your browser right after you'll see `Sample User created` message in the terminal window where rails task is running.

While you're waiting, please take the time to set up your [linter integration](./linters.md).

## Troubleshooting

### `rbenv: docker-sync: command not found`
Ensure you have `docker-sync` installed in the correct ruby version.

```sh
gem install docker-sync
```

### `ERROR: Cannot start service web: OCI runtime create failed: caused \\\"not a directory\\\"\"": unknown: Are you trying to mount a directory onto a file (or vice-versa)?`

Ensure that you have the correct permissions on the following folders:

```sh
$ sudo chmod a+x /usr/local/bin
$ which docker-compose # whatever is the resulting path, also do the above command
```

You may have to build twice, as the first time may fail.

### Docker Sync Clean
Sometimes weird errors may occur and docker-sync may be degraded or having issues. In order to start the ATS application again with a clean state without rebuilding, from the root dir of the checked out HiringThing ATS git repo run:

```sh
./atsdev clean # -c || --clean
```

This will perform the following on your local machine:
- clean docker-sync endpoints (`docker-sync clean`)
- stop the ATS application and remove all Docker containers running from the defined compose services (`docker-compose down`)

When the `clean` completes you will be left with the Docker images and networks but no built containers or running `docker-sync` daemon.

Run `./atsdev start` to use the ATS application again.

### Docker Purge
To remove all local Docker resources and start with a completely clean slate, from the root dir of the checked out HiringThing ATS git repo run:

```sh
./atsdev purge # -p || --purge
```

This will perform the following on your local machine:
- clean docker-sync endpoints locally
- remove any previously found ATS application Docker resources

When the `purge` completes you will be left with no ATS application Docker resources on your machine. Run `./atsdev build` again to start over.
