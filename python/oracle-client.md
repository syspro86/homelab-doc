# Oracle

## 설치

### 1. oracle instant client 설치가 필요

[https://www.oracle.com/database/technologies/instant-client.html](https://www.oracle.com/database/technologies/instant-client.html)

[https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html](https://www.oracle.com/database/technologies/instant-client/linux-x86-64-downloads.html)

[https://download.oracle.com/otn\_software/linux/instantclient/215000/instantclient-basiclite-linux.x64-21.5.0.0.0dbru.zip](https://download.oracle.com/otn_software/linux/instantclient/215000/instantclient-basiclite-linux.x64-21.5.0.0.0dbru.zip)

zip 압축 해제후, 압축해제 경로를 LD\_LIBRARY\_PATH 변수에 추가

```
export LD_LIBRARY_PATH=/opt/oracle/instantclient_21_5:$LD_LIBRARY_PATH
```

압축해제 경로 밑 network/admin/ 하위에 sqlnet.ora, tnsnames.ora 파일 복사

```
sudo yum install libaio   # centos fedora
sudo apt-get install libaio  # debian ubuntu
```

### 2. python module 설치

```
pip install cx-Oracle
```

### 3. 매뉴얼

[https://cx-oracle.readthedocs.io/en/latest/user\_guide/batch\_statement.html](https://cx-oracle.readthedocs.io/en/latest/user_guide/batch_statement.html)


### 4. 코드

모듈 초기화

```
import cx_Oracle
cx_Oracle.init_oracle_client(lib_dir=client_path)
```

DB 연결

```
conn = cx_Oracle.connect(user=user, password=password, dsn=dsn, encoding='UTF-8')
```

SQL 실행

```
cur = conn.cursor()

rows = cur.execute("select col1, col2 from table1 where col3 = :1 and col4 = :2", [val1, val2])
rows = cur.execute("select col1, col2 from table1 where col3 = :col3 and col4 = :col4", col3=val1, col4=val2)

if not rows:
    print('no result')
else:
    print(rows[0][0], rows[0][1])
    
    
cur.execute("insert into table1 (col1, col2, col3, col4) values (:1, :2, :3, :4)", val1, val2, val3, val4)
cur.executemany("insert into table1 (col1, col2, col3, col4) values (:1, :2, :3, :4)", [(val11, val12, val13, val14), (val21, val22, val23, val24)])


```