---
title: "[KEPCO] Constructing Demo System of AI based Diagnosis_1"
excerpt: "이건 뭐지?"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-22T12:24:00
---

### Goal
Oracle, HTML, Python을 활용한 데모를 만듬

### Background
회사에서 KEPCO랑 프로젝트를 진행중.
변전설비의 고장 진단을 AI 기반으로 함
한전에 축적된 데이터를 가지고 AI엔진을 돌려서 진단결과를 UI로 표출해줌. 

### Motivation
매번 내부망에 붙어서 패치할 수는 없잖아?
그래서 내부망을 모사한 사이트를 만들어두고 가서는 붙이기만 하려고함.

### Requirements
- Oracle 서버를 구축하고 원격(SQL Developer)으로 붙어서 데이터 관리
- Python을 이용해서 Oracle 데이터를 json format으로 변환.
- AI 엔진을 이용하여 산출된 결과를 Oracle에 저장
- AI 엔진을 이용하여 산출된 결과를 Web에서 확인

### Used Languagfe
- Docker
- Oracle
- Python
- Flask

### System Configuration
- 그림 넣을 예정

### HW SPECIFICATION
- Clinet: OS X
- Server: Linux (Ubuntu 20.04)

### Lists
1. Oracle Installation with Docker
2. SQL Developer on Mac M1