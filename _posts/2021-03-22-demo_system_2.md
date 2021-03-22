---
title: "[KEPCO] Constructing Demo System of AI based Diagnosis_2"
excerpt: "이건 뭐지?"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-22T12:24:00
---

### Intro
Linux에 Docker를 이용해 Oracle을 만들어보자

### Test Environments
- Linux ~~

### Docker Install

### Oracle Install
sudo docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1
(sudo 안주면 리소스 할당 못할수도 있음)
### Oracle sqlplus 확인
docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus /nolog"

### Oracle Volume 할당
docker run -d -it --name oracle_db -p 1521:1521 -v /data/sooho/oracle_volume store/oracle/database-enterprise:12.2.0.1
docker run -d -it --name oracle_db -p 1521:1521 store/oracle/database-enterprise:12.2.0.1

docker exec -it oracle_db bash -c "source /home/oracle/.bashrc; sqlplus sys/Oradoc_db1@ORCLCDB as sysdba"