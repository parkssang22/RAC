19.3에서 19.26으로 RU(Release Update)를 올리는 전형적인 RAC 패치 과정

[root@psk1 ~]# echo $GRID_HOME
/u01/app/grid/19c
[root@psk1 ~]# echo $ORACLE_HOME
/u01/app/oracle/product/19c


PSK#1> SELECT name, cdb, open_mode FROM v$database;

NAME      CDB OPEN_MODE
--------- --- --------------------
PSKDB     YES READ WRITE

PSK#1> SHOW pdbs;

    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 PDB                            MOUNTED

[oracle@psk1 ~]$ olsnodes -n
psk1    1
psk2    2

[oracle@psk1 ~]$ crsctl stat res -t

[oracle@psk1 ~]$ ls
app                              Downloads                       Pictures
asminit.ora                      LINUX.X64_193000_db_home.zip    p.sql
create_controlfile_20260414.sql  LINUX.X64_193000_grid_home.zip  Public
cre_controlfile.sql              log.sql                         Templates
demo.sql                         Music                           Videos
Desktop                          Oracle_Patch_19.26.zip
Documents                        owi

[oracle@psk1 ~]$ unzip /home/oracle/Oracle_Patch_19.26.zip


[oracle@psk1 ~]$ cd /home/oracle/Oracle_Patch_19.26/Opatch
[oracle@psk1 Opatch]$ unzip p6880880_190000_Linux-x86-64.zip

[oracle@psk1 Opatch]$ cd ../RU
[oracle@psk1 RU]$ unzip p37257886_190000_Linux-x86-64.zip









[root@psk1 ~]# export GRID_HOME=/u01/app/grid/19c
[root@psk1 ~]# export ORACLE_HOME=/u01/app/oracle/product/19c
[root@psk1 ~]# mv $GRID_HOME/OPatch $GRID_HOME/OPatch_old
[root@psk1 ~]# mv $ORACLE_HOME/OPatch $ORACLE_HOME/OPatch_old
[root@psk1 ~]# cp -r /home/oracle/Oracle_Patch_19.26/Opatch/OPatch $GRID_HOME
[root@psk1 ~]# cp -r /home/oracle/Oracle_Patch_19.26/Opatch/OPatch $ORACLE_HOME


[root@psk1 ~]# vi .bash_profile
[root@psk1 ~]# cat .bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH

export ORACLE_BASE=/u01/app/oracle;
export GRID_HOME=/u01/app/grid/19c;
export ORACLE_HOME=$ORACLE_BASE/product/19c;
export PATH=$PATH:$GRID_HOME/bin;
# 아래 2개의 내용 추가
export CV_ASSUME_DISTID=OEL8.9   
export ORACLE_SID=pskdb
[root@psk1 ~]#





```bash
[root@psk1 ~]# $GRID_HOME/OPatch/opatchauto version

Invalid current directory.  Please run opatchauto from other than '/root' and '/' directory.
And check if the home owner user has write permission set for the current directory.
opatchauto returns with error code = 2

[root@psk1 ~]# $ORACLE_HOME/OPatch/opatchauto version

Invalid current directory.  Please run opatchauto from other than '/root' and '/' directory.
And check if the home owner user has write permission set for the current directory.
opatchauto returns with error code = 2
```

- 다음과 같이 에러뜨면 
[root@psk1 ~]# cd /tmp
[root@psk1 tmp]# $GRID_HOME/OPatch/opatchauto version
Oracle OPatchAuto 버전 12.2.1.46.0
Copyright (c) 2016, Oracle Corporation. All rights reserved.

1. OPatchAuto 버전 12.2.1.46.0
2. OpatchautoDB 버전 12.2.0.1.46

[root@psk1 tmp]# $ORACLE_HOME/OPatch/opatchauto version
Oracle OPatchAuto 버전 12.2.1.46.0
Copyright (c) 2016, Oracle Corporation. All rights reserved.

1. OPatchAuto 버전 12.2.1.46.0
2. OpatchautoDB 버전 12.2.0.1.46

- opatchauto는 root와 oracle이 사용하는데 /root 폴더는 oracle 사용자가 접근할 수 없기에 에러가 뜨기에 모든 사용자가 사용할 수 있는 tmp 폴더에서 실행한다


## 같은 방법으로 opatch 버전도 확인한다
- 버전이 일치해야 한다

[root@psk1 tmp]# $GRID_HOME/OPatch/opatch version
OPatch Version: 12.2.0.1.46

OPatch succeeded.
[root@psk1 tmp]# $ORACLE_HOME/OPatch/opatch version
OPatch Version: 12.2.0.1.46

OPatch succeeded.








[root@psk1 tmp]# cd $GRID_HOME/OPatch
[root@psk1 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME







[root@psk1 tmp]# cd $GRID_HOME/OPatch
[root@psk1 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME

OPatchauto session is initiated at Thu Apr 30 03:03:38 2026
OPATCHAUTO-72083: 부트스트랩 작업 수행을 실패했습니다.
OPATCHAUTO-72083: 부트스트랩 실행을 실패했습니다. 원인: Failed to unzip files on path./home/oracle/Oracle_Patch_19.26/RU/37257886/37260974/files/perl.zipError::Can't open perl script "/u01/app/grid/19c//OPatch/auto/database/bin/ZipUnzip.pl": (null)
.
OPATCHAUTO-72083: 보고된 문제를 수정하고 opatchauto를 다시 실행하십시오.
com.oracle.glcm.patch.auto.OPatchAutoException: OPATCHAUTO-72083: 부트스트랩 작업 수행을 실패했습니다.
OPATCHAUTO-72083: 부트스트랩 실행을 실패했습니다. 원인: Failed to unzip files on path./home/oracle/Oracle_Patch_19.26/RU/37257886/37260974/files/perl.zipError::Can't open perl script "/u01/app/grid/19c//OPatch/auto/database/bin/ZipUnzip.pl": (null)
.
OPATCHAUTO-72083: 보고된 문제를 수정하고 opatchauto를 다시 실행하십시오.
        at com.oracle.glcm.patch.auto.db.util.BootstrapHandler.performBootstrapping(BootstrapHandler.java:542)
        at com.oracle.glcm.patch.auto.db.util.BootstrapHandler.main(BootstrapHandler.java:98)

OPatchauto session completed at Thu Apr 30 03:03:54 2026
Time taken to complete the session 0 minute, 17 seconds

opatchauto bootstrapping failed with error code 255.

- 에러뜨면 

[root@psk1 OPatch]# export GRID_HOME=/u01/app/grid/19c
[root@psk1 OPatch]# export ORACLE_HOME=/u01/app/oracle/product/19c
# 다시 한번 소유권을 oracle에게 확실히 부여
[root@psk1 OPatch]# chown -R oracle:oinstall /u01/app/grid/19c/OPatch
[root@psk1 OPatch]# chown -R oracle:oinstall /u01/app/oracle/product/19c/OPatch
# 모든 스크립트에 실행 권한 부여
[root@psk1 OPatch]# chmod -R 755 /u01/app/grid/19c/OPatch
[root@psk1 OPatch]# chmod -R 755 /u01/app/oracle/product/19c/OPatch
# 다시 실행
[root@psk1 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME






[root@psk1 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME

OPatchauto session is initiated at Thu Apr 30 03:09:39 2026

System initialization log file is /u01/app/grid/19c/cfgtoollogs/opatchautodb/systemconfig2026-04-30_03-09-59AM.log.

세션 로그 파일은 /u01/app/grid/19c/cfgtoollogs/opatchauto/opatchauto2026-04-30_03-12-44AM.log입니다.
이 세션의 ID는 8F4T입니다.

Wrong OPatch software installed in following homes:
Host:psk2, Home:/u01/app/oracle/product/19c

Host:psk2, Home:/u01/app/grid/19c

OPATCHAUTO-72088: OPatch 버전 검사를 실패했습니다.
OPATCHAUTO-72088: 패치 작업을 위해 선택된 홈의 OPatch 소프트웨어 버전이 서로 다릅니다.
OPATCHAUTO-72088: 모든 홈에 동일한 OPatch 소프트웨어를 설치하십시오.
OPatchAuto를 실패했습니다.

OPatchauto session completed at Thu Apr 30 03:13:56 2026
Time taken to complete the session 3 minutes, 59 seconds

 opatchauto failed with error code 42

- 에러뜨면 2번 노드에서도 opatch 업데이트

[oracle@psk2 ~]$ ls
datafile.sql  Desktop  Documents  Downloads  Music  Oracle_Patch_19.26.zip  Pictures  Public  Templates  Videos
[oracle@psk2 ~]$ unzip /home/oracle/Oracle_Patch_19.26.zip


[oracle@psk2 ~]$ cd /home/oracle/Oracle_Patch_19.26/Opatch
[oracle@psk2 Opatch]$ unzip p6880880_190000_Linux-x86-64.zip

[oracle@psk2 RU]$ cd ../RU
[oracle@psk2 RU]$ unzip p37257886_190000_Linux-x86-64.zip



[root@psk2 ~]# mv $GRID_HOME/OPatch $GRID_HOME/OPatch_old

[root@psk2 ~]# mv $ORACLE_HOME/OPatch $ORACLE_HOME/OPatch_old

[root@psk2 ~]# cp -r /home/oracle/Oracle_Patch_19.26/Opatch/OPatch $GRID_HOME
[root@psk2 ~]# cp -r /home/oracle/Oracle_Patch_19.26/Opatch/OPatch $ORACLE_HOME





[root@psk2 ~]# cat .bash_profile
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH

export ORACLE_BASE=/u01/app/oracle;
export GRID_HOME=/u01/app/grid/19c;
export ORACLE_HOME=$ORACLE_BASE/product/19c;
export PATH=$PATH:$GRID_HOME/bin;
export CV_ASSUME_DISTID=OEL8.9
export ORACLE_SID=pskdb




[root@psk2 tmp]# $GRID_HOME/OPatch/opatchauto version
Oracle OPatchAuto 버전 12.2.1.46.0
Copyright (c) 2016, Oracle Corporation. All rights reserved.

1. OPatchAuto 버전 12.2.1.46.0
2. OpatchautoDB 버전 12.2.0.1.46

[root@psk2 tmp]# $ORACLE_HOME/OPatch/opatchauto version
Oracle OPatchAuto 버전 12.2.1.46.0
Copyright (c) 2016, Oracle Corporation. All rights reserved.

1. OPatchAuto 버전 12.2.1.46.0
2. OpatchautoDB 버전 12.2.0.1.46

[root@psk2 tmp]# $GRID_HOME/OPatch/opatch version
OPatch Version: 12.2.0.1.46

OPatch succeeded.
[root@psk2 tmp]# $ORACLE_HOME/OPatch/opatch version
OPatch Version: 12.2.0.1.46

OPatch succeeded.



## Grid Home, Oracle Home의 OPatch 권한 부여
[root@psk2 tmp]# chown -R oracle:oinstall /u01/app/grid/19c/OPatch
[root@psk2 tmp]# chmod -R 755 /u01/app/grid/19c/OPatch
[root@psk2 tmp]# chown -R oracle:oinstall /u01/app/oracle/product/19c/OPatch
[root@psk2 tmp]# chmod -R 755 /u01/app/oracle/product/19c/OPatch




[root@psk1 OPatch]# cd $GRID_HOME/OPatch
[root@psk1 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME


## 패치 완료 후 확인한다

[oracle@psk1 OPatch]$ $GRID_HOME/OPatch/opatch lspatches
37268031;OCW RELEASE UPDATE 19.26.0.0.0 (37268031)
37260974;Database Release Update : 19.26.0.0.250121 (37260974)

OPatch succeeded.
[oracle@psk1 OPatch]$ $ORACLE_HOME/OPatch/opatch lspatches
37268031;OCW RELEASE UPDATE 19.26.0.0.0 (37268031)
37260974;Database Release Update : 19.26.0.0.250121 (37260974)


## 2번 노드도 똑같이 실행한다
[root@psk2 ~]# cd $GRID_HOME/OPatch
[root@psk2 OPatch]# $GRID_HOME/OPatch/opatchauto apply /home/oracle/Oracle_Patch_19.26/RU/37257886 -oh $GRID_HOME,$ORACLE_HOME

[oracle@psk2 ~]$ $GRID_HOME/OPatch/opatch lspatches
37268031;OCW RELEASE UPDATE 19.26.0.0.0 (37268031)
37260974;Database Release Update : 19.26.0.0.250121 (37260974)

OPatch succeeded.
[oracle@psk2 ~]$ $ORACLE_HOME/OPatch/opatch lspatches
37268031;OCW RELEASE UPDATE 19.26.0.0.0 (37268031)
37260974;Database Release Update : 19.26.0.0.250121 (37260974)

OPatch succeeded.


[oracle@psk2 ~]$ sqlplus / as sysdba

SQL*Plus: Release 19.0.0.0.0 - Production on Sun May 3 23:12:08 2026
Version 19.26.0.0.0

Copyright (c) 1982, 2024, Oracle.  All rights reserved.


Connected to:
Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.26.0.0.0
