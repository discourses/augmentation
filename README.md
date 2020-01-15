branch|state
:---|:---
develop|![](https://github.com/greyhypotheses/augmentation/workflows/Derma%20Python%20Package/badge.svg?branch=develop)
master|![](https://github.com/greyhypotheses/augmentation/workflows/Derma%20Python%20Package/badge.svg?branch=master)

<br>

# Augmentation

This repository complements the

* [derma](https://github.com/greyhypotheses/derma)
* [dermatology](https://github.com/greyhypotheses/dermatology)

repositories. It creates augmentations of the original images of [dermatology](https://github.com/greyhypotheses/dermatology) for the models of [derma](https://github.com/greyhypotheses/derma).  This is a critical step because the models of [derma](https://github.com/greyhypotheses/derma) include deep transfer learning models that require specific image dimensions.  Upcoming updates to this prospective package are

* Runtime image dimensions argument, i.e., the ability to input a tuple of the required width & height at runtime.
* Non-mandatory angle of rotation; presently, this must be specified in the global variables dictionary.
* Online based [YAML global dictionaries](./src/federal).
* Missing tests.

<br>
<br>

## Steps

Each image is transformed according to the steps of [generator.Generator().augment(...)](./src/data/generator.py)

<br>
<br>

## Running

The augmentation algorithms of this repository are ran via a container of a Docker image.  The image is created by GitHub Actions using this repository's [Dockerfile](./Dockerfile), and automatically pushed to Docker Hub section [greyhypotheses/derma:augmentation](https://hub.docker.com/r/greyhypotheses/derma/tags).

Thus far the image has been pulled & ran within an Amazon EC2 Linux machine:
  * Amazon Linux AMI 2018.03.0.20190826 x86_64 HVM gp2
  * amzn-ami-hvm-2018.03.0.20190826-x86_64-gp2 (ami-00eb20669e0990cb4)

<br>

### Via a Docker Container

In the code snippet below, the required image is *pulled* from Docker Hub after ascertaining that docker is running.

#### Is docker running?

```bash
# Update the environment
sudo yum update -y

# Install Docker
sudo yum install -y docker
docker --version
sudo service docker start

# In order to use docker commands without 'sudo'
sudo usermod -a -G docker ec2-user
exit

# Login again
ssh -i ***.pem ec2-user@**.**.***.**

# Hence
docker info

```

<br>

#### Hence, pull the image

```bash

# Pull image
docker pull greyhypotheses/derma:augmentation

```

<br>

#### Run a container

Running a container of the image, as outlined below, runs the algorithms of this repository.  The resulting images are zipped.  If access to a cloud repository is available, a method that automatically transfers the files to the cloud repository can be added to [main.py](./src/main.py).

```bash
# Container
# Help: https://docs.docker.com/engine/reference/commandline/run/
# -v ~/images:/app/images => mapping local path ~/images to the volume of the container, i.e., /app/images
# -d => run the container in the background
docker run -d -v ~/images:/app/images greyhypotheses/derma:augmentation

# Thus far, how many images?
cd images
ls | wc -l
```

<br>

#### Download Option

Case local:

```bash
scp -i ***.pem ec2-user@**.**.***.**:~/images/*.csv augmentation/images/
scp -i ***.pem ec2-user@**.**.***.**:~/images/*zip augmentation/images/
```

<br>
<br>

### Docker Help Notes


#### Clearing Docker Containers
```bash
# -v ensures that associated volumes are also deleted
docker rm -v ... [container code]
```

#### Clearing Docker Volumes

```bash
# List volumes
docker volume ls

# Delete all volumes
docker volume rm $(docker volume ls -q)
```

#### Clearing Docker Images
```bash
# -v ensures that associated volumes are also deleted
docker rmi ... [image code]
```
