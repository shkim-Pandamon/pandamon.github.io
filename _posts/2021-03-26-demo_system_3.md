---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_3"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - projects
tages:
    - projects
last_modified_at: 2020-03-26T12:24:00
---

## Goal
> To construct `Oracle` server using `Docker`
> <small>*Docker를 이용해 Oracle Server를 구축해보자*</small>

## System Specification
This project uses following HW resources.   
<small>*본 프로젝트에 사용된 HW 리소스는 다음과 같습니다.*</small>  

> - Oracle Server: Linux (`Ubuntu 20.04`) <span style="color:red">(Today's Use)</span>
> - AI Server: Linux (`Ubuntu 18.04`)
> - Client Server: macOS (`Big sur`) on M1 chip

## Intro
As decribed in previous post, I'm gonna use `Docker` container to use `Oracle DB` in this post. Recently it is observed that the usage of `Docker` becomes universal, and clear evidence is that many famous SWs such as `Oracle DB` provide their own Docker images. Thus, we do not need to make docker image ourselves, but we can just download them from [`Docker Hub`](https://hub.docker.com/). First, search `Oracle` at [`Docker Hub`](https://hub.docker.com/), and you can see the results as below.  
<small>*저번 포스트에서 설명드린 이유로, 본 포스트에서는 도커 컨테이너를 이용하여 오라클을 돌리는 내용을 다루도록 하겠습니다. 최근 도커의 사용이 점차 보편화되고 있는것이 느껴지는데요, 그 대표적인 예시가 오라클을 포함한 많은 SW가 도커 이미지를 제시한다는 점입니다. 따라서 예전처럼 도커 이미지를 직접 말아서(한국에서는 "만다"라는 표현을 많이 쓰는 것 같더군요) 도커 컨테이너를 올릴필요 없이 [도커 허브](https://hub.docker.com/)에 이미 오라클에서 직접 올린 오라클 이미지를 찾을 수 있습니다. [도커 허브](https://hub.docker.com/)에서 `oracle`을 검색해보면 아래와 같이 나옵니다.*</small> 

<img src="/images/2021-03-22-demo_system_2_fig1.png" alt="drawing" width="600"/>

I'm gonna use `Oracle Database Enterprise Edition`. To use this image, click it and proceed to checkout. Then you can see the below.  
<small>*우린 여기서 `Oracle Database Enterprise Edition`을 활용하도록 하겠습니다. 해당 이미지를 사용하기 위해서는 로그인 후 Checkout를 진행하셔야 합니다. Checkout을 마치면 다음의 화면을 보실 수 있습니다.*</small>

<img src="/images/2021-03-22-demo_system_2_fig2.png" alt="drawing" width="600"/>  

You can just follow the procedure described in the above page, but in my case I prefer little additional procedure: Port-forwarding and Volume usage.  
<small>*여기에 나오는 설명대로 진행하셔도 큰 무리 없이 설치를 진행하실 수 있겠지만, 저는 조금의 추가 작업을 진행하였는데요, 전체적인 과정을 본격적으로 설명드리도록 하겠습니다.*</small>

## Oracle Installation
I used `slim version`. The difference of `default` and `slim` version is if it offers *"Analytics, Oracle R, Oracle Label Security, Oracle Text, Oracle Application Express and Oracle DataVault."* or not. Since I just construct demo server and it does not require above functions, I prefer `slim version` which is lighter to use. you can use `slim version` by add '-slim' to command without space.  
<small>*저는 slim 버전을 사용하였습니다. default 버전과 slim 버전의 차이는 "Analytics, Oracle R, Oracle Label Security, Oracle Text, Oracle Application Express and Oracle DataVault."의 지원여부인데요, 저의 경우 데모서버 수준에서는 굳이 필요하지 않은 기능이어서 저는 좀더 가벼운 slim버전으로 최종 선택하였습니다. slim버전을 사용하기 위해서는 명령어 뒤에 '-slim'을 스페이스 없이 추가해주면 됩니다.*</small>

```bash
# login to docker hub first
docker login

# download docker image and run it
docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1-slim
```

`-p` it an option for port forwarding. I assigned 1521 as port.   
<small>*이떄 '-p' 옵션은 포트포워딩을 위한 옵션입니다. 저의 경우 1521 포트를 할당하였습니다.*</small>

After installation you can check the presence of the image and the container by commands `docker images -a` and `docker ps -a`. If the status of container show as `healthy`, then the installation was done successfully.  
<small>*설치가 끝나면 'docker images -a'와 'docker ps -a'를 통해 각각 이미지와 컨테이너의 존재여부를 확인할 수 있습니다. 이때 컨테이너의 상태가 아래와 같이 'healthy'일 경우 설치가 잘 완료되었다고 판단할 수 있습니다.*</small>

<img src="/images/2021-03-22-demo_system_2_fig3.png" alt="drawing" width="600"/>

Now, Let's configure if `Oracle` work well using `sqlplus`. Put command as below and see how the result would come.
<small>*이제 'sqlplus'를 이용해 오라클이 잘 돌아가고 있는지 확인해보도록 하겠습니다. 먼저 아래와 같이 입력했을때 어떤 결과가 오는지 확인해봐야합니다.*</small>

```bash
# First, get into the container
docker exec -it oracle_db bash

# If you become in container, open sqlplus
source /home/oracle/.bashrc
sqlplus /nolog

# IF you want to open sqlplus with sysdba account then
sqlplus
SQL> /as sysdba

# or you can directly open sqlplus in the container by
docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus /nolog"
```

If you see result as below, your `Oracle` works well.   
<small>*만약 아래와 같이 나온다면 성공입니다.*</small>

<img src="/images/2021-03-22-demo_system_2_fig4.png" alt="drawing" width="600"/>

The ID and PW of initial account setup are `sys` and `Oradoc_db1` respectively, and SID of DB is `ORCLCDB`. When you want to get out from `sqlplus` or `container bash`, just use a command `exit`  
<small>*기본적으로 세팅되어 있는 dba권한의 ID와 PW는 각각 'sys'와 'Oradoc_db1'입니다. 또 DB의 SID는 ORCLCDB로 설정되어 있습니다. 'sqlplus' 및 'container bash'에서 나올때는 'exit' 명령어를 사용해서 나올 수 있습니다.*</small>

As a last, you can set up the volume for the `Oracle Container`. Because of the characteristic of DB, `Oracle Contianer` could store important and huge amount of data. If a volume is not allocated to the `Oracle Container`, the data could be lost or damaged when problem occurs in the operation of `Oracle Container`. To prevent is we can allocate the local directory as a volume to `Oracle Container`, and it could be simply done by `-v` option when create the container.   
<small>*마지막으로 볼륨을 설정해봅시다. 오라클 컨테이너는 DB 특성상 많은 데이터를 저장하게 되는데, 따로 볼륨을 지정하지 않을 경우 컨테이너에 오류가 발생하거나 컨테이너 동작이 멈출경우 데이터도 함께 손실될 위험이 있습니다. 이를 방지하기 위해 컨테이너와 분리된 로컬 디렉토리를 볼륨으로 지정하여 데이터를 보존할 수 있는데, 단지 컨테이너 생성시 로컬의 볼륨 디렉토리를 지정해주기만 하면 됩니다.*</small>

``` bash
# use '-v' option
docker run -d -it --name oracle_db -p 1521:1521 -v [~/volume_directory] store/oracle/database-enterprise:12.2.0.1-slim

## example
docker run -d -it --name oracle_db -p 1521:1521 -v /docker_volume/oracle store/oracle/database-enterprise:12.2.0.1-slim
```