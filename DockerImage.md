# NSML Docker Custom Image 만드는 방법

1. [NSML Team](https://ai.nsml.navercorp.com/)은 총 9개의 기본 Docker Image을 제공합니다. [(바로가기)](https://hub.docker.com/u/nsml)
2. 각각의 이미지는 자신의 `Model` 환경에 맞춰 사용합니다.
3. [NSML Team](https://ai.nsml.navercorp.com/)에서 제공한 Docker Image이외 사용해야할 `python library`가 있는 경우 사용할 `library`를 `requirements.txt`파일에 명시하여 [Docker Image](#dockerfile-예시)을 만들 수 있습니다.

   ```text
   # requirements.txt Example
   pandas
   numpy
   konlpy
   scikit-learn
   attention
   urllib3
   argparse
   glob2
   rouge-metric
   utility
   librosa
   ffmpeg
   transformers
   tensorflow-gpu==2.4.0
   ...
   ```

4. `requirements.txt`파일의 경우 `$ pip3 install <PYTHON_LIBRARY_NAME>`에서 사용하는 library name 값을 사용해야 정상 설치가 됩니다. [pip 설치 참고](https://pypi.org/)

   ```shell
   # glob 설치시 사용하는 pip 설치 명령어
   $ pip install glob2 (o)
   $ pip install glob (x)

   ```

# Dockerfile 예시

1. 사용자가 사용할 python libary가 있는 경우 `dockerfile`과 `requiremonts.txt`파일은 같은 디렉터리 안에 있어야 합니다.

```shell
$ tree nia-docker
   nia-docker
   ├── Dockerfile
   └── requirements.txt
```

## Dockerfile Example

```dockerfile
# Docker Base Image
FROM nvcr.io/nvidia/tensorflow:20.06-tf2-py3
MAINTAINER  dacon_DavTeam <dacon@dacon.io>

# python lib
COPY requirements.txt ./

# pip3 install and apt-get update
RUN apt-get update && apt-get install -y vim libbz2-dev python3-pip g++ openjdk-8-jdk
RUN pip install --upgrade pip

# install Python Packages
RUN pip install -r requirements.txt
```

# Docker Image Build

```shell
# dockerhub login (docker image을 push하기 위함)
$ docker login

# docker fild build
$ docker build -t daconDevTeam/nsml-nia-competiton .

# docker iamge push to dockerhub
docker push daconDevTeam/nsml-nia-competiton

```

# 참고자료

1. Docker 설치 방법 [Docker Document](https://docs.docker.com/get-docker/)

```shell
# window
$ choco install docker

# macos
$ brew install --cask docker

# linux(ubuntu)
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
$ sudo apt-cache madison docker-ce
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
```
