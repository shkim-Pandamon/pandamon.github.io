---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_2"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-22T12:24:00
---

## Goal
> To construct `Oracle` server using `Docker`
> <small>*Docker를 이용해 Oracle Server를 구축해보자*</small>

## SYSTEM SPECIFICATION
This project uses following HW resources.   
<small>*본 프로젝트에 사용된 HW 리소스는 다음과 같습니다.*</small>

> - Oracle Server: Linux (`Ubuntu 20.04`) (Today's Use)
> - AI Server: Linux (`Ubuntu 18.04`)
> - Test Server: OS X(`Big sur`) on M1 chip


## Oracle Install
docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1-slim

## Oracle sqlplus 확인
docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus /nolog"

## Oracle Volume 할당
docker run -d -it --name oracle_db -p 1521:1521 -v /data/sooho/oracle_volume store/oracle/database-enterprise:12.2.0.1-slim


docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus sys/Oradoc_db1@ORCLCDB as sysdba"