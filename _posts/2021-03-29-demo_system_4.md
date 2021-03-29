---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_4"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-29T12:24:00
---

## Goal
> To build up a tablespace and account on `Oracle DB`   
> <small>*오라클 데모 환경을 세팅하자: Tablespace와 계정 설정*</small>

## System Specification
This project uses following HW resources.   
<small>*본 프로젝트에 사용된 HW 리소스는 다음과 같습니다.*</small>  

> - Oracle Server: Linux (`Ubuntu 20.04`)  <span style="color:red">(Today's Use)</span>
> - AI Server: Linux (`Ubuntu 18.04`)
> - Test Server: macOS (`Big sur`) on M1 chip

## Intro
In usual, a `tablespace` and an `account` would be allocated to each project when use `Oracle DB`. The user can access to the data with his account and the allocated tablespace. In this post, I am gonna make a new tablespace and allocate it to new user account.  
<small>*일반적으로 오라클 DB 사용에는 각 프로젝트별 테이블스페이스와 계정을 할당해주게됩니다. 사용자는 각 계정과 그 계정에 할당된 테이블스페이스에서 데이터를 접근할 수 있습니다. 이번 포스트에서는 테이블스페이스를 만들고 새로운 계정에 테이블스페이스를 할당해보도록 하겠습니다.*</small>

## Making Tablespace
First, I am gonna make a directory to store the data of a new tablespace. Since `Oracle DB` is running on `Docker container`, we need to get into the container and make a directory that we want.   
<small>*먼저 새로운 테이블스페이스의 데이터를 저장할 디렉토리를 생성해주도록 하겠습니다. 현재 오라클 DB가 도커에서 돌아가고 있으므로 먼저 도커에 들어가서 원하는 디렉토리를 생성해야합니다.*</small>

```bash
# First, get into the docker container.
docker exec -it <container_name> bash
# example) docker exec -it oracle_db bash

# make a new directory that you will use.
mkdir <new_directory>

# example)
# cd /home/oracle
# mkdir oradata
# cd oradata
# mkdir ORCLCDB
```

Next, Let's make a new tablespace on `Oracle DB`. Put bellow commands to `Oracle DB` with `sysdba` account at `SQL Plus` or `SQL Developer`.   
<small>*다음으로 오라클에서 테이블 스페이스를 만들어주도록 합니다. SQL Plus, SQL Developer 상관없이 어디에서든 sysdba 권한으로 로그인해서 아래의 명령어를 오라클 DB에 날려주도록 합시다. *</small>

```sql
create tablespace <table_space_name>
datafile '/home/oracle/oradata/ORCLCDB/seda_db.dbf' size 10240M;
```

In my case, since it is demo system, I only allocated storage size 10Gb.  
<small>*저의 경우 데모 시스템인만큼 너무 많은 용량을 할당하기 보다는 10Gb정도의 용량을 할당해주었습니다.*</small>

## User
Now, let's create new user. There is a small issue that from the `Oracle 12c`, it is required to attache `C##` in fromt of the username, or the error would occure with the message that `Invalid common user or role name`. Since it is inconvenient, remove this setting first.  
<small>*다음으로 User를 생성해주도록 하겠습니다. 먼저 주의해야할 것이 Oracle 12c부터는 유저 이름 앞에 'C##'을 붙여야지 'Invalid common user or role name'라는 에러 없이 유저 생성이 가능한데요, 아무래도 불편할 수 있으니 이러한 설정을 먼저 해제하도록 하겠습니다.*</small>

```sql
alter session set "_ORACLE_SCRIPT"=TRUE;
```

To create new user, put the username, password, and the tablespace that the user would be in.  
<small>*유저 계정 생성을 위해서는 생성할 유저의 이름, 비밀번호, 유저가 속한 테이블스페이스 지정이 필요합니다.*</small>

```sql
CREATE USER "<name>" IDENTIFIED BY "<password>"  
DEFAULT TABLESPACE "<table_space_name>"
TEMPORARY TABLESPACE "TEMP";
```

As a last, allocate the authority for DB access and modification to the created user.  
<small>*마지막으로 생성한 유저에게 DB 접속 및 수정 권한을 할당합니다.*</small>

```sql
GRANT CONNECT,RESOURCE,DBA TO "<name>";
GRANT CREATE TABLE, CREATE VIEW TO "<name>";
GRANT CONNECT,DBA TO "<name>";
```