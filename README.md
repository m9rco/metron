<p align="center">
<img src="logo.png" width=300/>
</p>

# Apache Metron

<p align="center">
  
[![Author](https://img.shields.io/badge/Author-m9rco-Green.svg?style=flat&logo=tinder)](https://about.me/pushaowei/)
[![Open Source Love](https://badges.frapsoft.com/os/v2/open-source.svg?v=102)](https://travis-ci.org/m9rco/metron/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://github.com/m9rco/metron/pulls)
[![Build Status](https://travis-ci.org/m9rco/metron.svg?branch=master)](https://travis-ci.org/m9rco/metron) 
![Docker Pulls](https://img.shields.io/docker/pulls/m9rco/metron.svg?style=flat-square)

</p>

## Images

- [0.7.1](0.7.1)


## Usage

The recommended way to get the m9rco metron Docker Image is to pull the prebuilt image from the [Docker Hub Registry](https://hub.docker.com/r/m9rco/metron).

```sh
docker pull m9rco/metron:latest
```

To use a specific version, you can pull a versioned tag. You can view the [list of available versions](https://hub.docker.com/r/m9rco/metron/tags/) in the Docker Hub Registry.

```sh
$ docker pull m9rco/metron:[TAG]
```

## Run the Docker

```
docker run -itd --privileged --name=metron m9rco/metron
```

## Go inside the docker

```sh
docker exec -it --privileged metron bash
```

## Build Metron

```
cd ~/metron
mvn clean package -DskipTests [-e -X]
```

### Further documentation
For further documentation, please check metron documentation or its GitHub repository

### Contributing
We'd love for you to contribute to this container. You can request new features by creating an issue, or submit a pull request with your contribution.    
