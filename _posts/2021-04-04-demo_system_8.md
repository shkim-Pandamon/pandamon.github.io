---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_7"
excerpt: "Let's build up a demo server of KEPCO SEDA system"

categories:
    - projects
tages:
    - projects
last_modified_at: 2020-04-04T12:24:00
---

## Goal
> To deliver the results using `Gunicorn`   
> <small>*Gunicorn을 이용해서 Web Server에 결과값을 전달해보자*</small>

## System Specification
This project uses following HW resources.   
<small>*본 프로젝트에 사용된 HW 리소스는 다음과 같습니다.*</small>  

> - Oracle Server: Linux (`Ubuntu 20.04`)
> - AI Server: Linux (`Ubuntu 18.04`)  <span style="color:red">(Today's Use)</span>
> - Client Server: macOS (`Big sur`) on M1 chip

## WSGI?