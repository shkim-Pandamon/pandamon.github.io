---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_4"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-22T12:24:00
---

## Intro
Oracle Setup

## Tablespace
docker exec -it oracle_db bash
cd /home/oracle
mkdir oradata
cd oradata
mkdir ORCLCDB

create tablespace SEDA_DB
datafile '/home/oracle/oradata/ORCLCDB/seda_db.dbf' size 10240M;

## User
12c부터 C##이 붙어야함 안그러면 에러메세지 표출
invalid common user or role name

alter session set "_ORACLE_SCRIPT"=TRUE;

CREATE USER "SEDA_DEV" IDENTIFIED BY "hmi@seda1234!!"  
DEFAULT TABLESPACE "SEDA_DB"
TEMPORARY TABLESPACE "TEMP";

GRANT CONNECT,RESOURCE,DBA TO "SEDA_DEV";
GRANT CREATE TABLE, CREATE VIEW TO "SEDA_DEV";
GRANT CONNECT,DBA TO "SEDA_DEV";

## Table


## from DB 2 DB