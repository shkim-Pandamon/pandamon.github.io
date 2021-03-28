---
title: "[KEPCO] Constructing Demo System for AI based Diagnosis_5"
excerpt: "이건 뭐지?"

categories:
    - Blog
tages:
    - Blogs
last_modified_at: 2020-03-22T12:24:00
---

## Intro
cx_Oracle 이용

## cx_Oracle
pip 설치, 오류 발생
Client


>To get the libraries:

>If your database is on a remote computer, then download and unzip the client libraries from the free Oracle Instant Client “Basic” or “Basic Light” package for your operating system architecture.
>
>Instant Client on Windows requires an appropriate Microsoft Windows Redistributables, see Installing cx_Oracle on Windows. On Linux, the libaio (sometimes called libaio1) package is needed. Oracle Linux 8 also needs the libnsl package.
>
>Alternatively, use the client libraries already available in a locally installed database such as the free Oracle Database Express Edition (“XE”) release.
>
>Version 19, 18 and 12.2 client libraries can connect to Oracle Database 11.2 or greater. Version 12.1 client libraries can connect to Oracle Database 10.2 or greater. Version 11.2 client libraries can connect to Oracle Database 9.2 or greater.


ZIP으로 클라이언트 받고 설치
- 다운로드 :
>curl -O https://download.oracle.com/otn_software/linux/instantclient/199000/instantclient-basic-linux.x64-19.9.0.0.0dbru.zip
- 설치
mkdir -p /opt/oracle
unzip instantclient-basic-linux.x64-21.1.0.0.0.zip -d /opt/oracle/

sudo sh -c "echo /opt/oracle/instantclient_19_9 > /etc/ld.so.conf.d/oracle-instantclient.conf"
sudo ldconfig

LOCATION = '/opt/oracle/instantclient_19_9'
cx_Oracle.init_oracle_client(lib_dir=LOCATION)

host = cx_Oracle.makedsn('10.10.20.54',1521,'ORCLCDB') # New D
id = 'SEDA_DEV'
pw = 'hmi@seda1234!!'
conn = cx_Oracle.connect(id, pw, host)


## 한글이 문제로구나
sql
update sys.props$ set value$='AMERICAN' where name='NLS_LANGUAGE';
update sys.props$ set value$='AMERICA' where name='NLS_TERRITORY';
update sys.props$ set value$='KO16KSC5601' where name='NLS_CHARACTERSET';
update sys.props$ set value$='AL16UTF16' where name='NLS_NCHAR_CHARACTERSET';

restart oracle
[oracle] lsnrctl stop
[oracle] sqlplus /nolog
SQL> connect /as sysdba
SQL> shutdown immediate;
SQL> startup;
[oracle] lsnrctl start






https://ending1.tistory.com/14
https://blog.naver.com/jjg2510/10155894448