---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_1"
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

## Test Environments
- Linux ~~

## Docker Install


## Oracle Install
docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1

## Oracle sqlplus 확인
docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus /nolog"

## Oracle Volume 할당
docker run -d -it --name oracle_db -p 1521:1521 -v /data/sooho/oracle_volume store/oracle/database-enterprise:12.2.0.1
docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1

docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus sys/Oradoc_db1@ORCLCDB as sysdba"