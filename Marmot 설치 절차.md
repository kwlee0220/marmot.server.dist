## Marmot 서버 배포판 설치 방법

### 1. 설치 사전 조건
* Oracle Java를 (Java8 이상) 설치되어 있어야 한다.
* Gradle이 설치되어 있어야 한다.
	- 참고:  http://sdkman.io/install.html

### 2. 설치 절차
1. Home 디렉토리 아래에 'marmot'이라는 디렉토리를 생성한다.
> `$ cd`
> `$ mkdir marmot`

2. 압축을 풀어 생성된 디렉토리를 생성된 곳을 이동시킨다.
>`$ cd marmot`

3. GitHub에서 Marmot server 배포판을 download한다.
* URL 주소: `https://github.com/kwlee0220/marmot.server.dist`

4. Download받은 zip 파일 (`marmot.server.master.dist.zip`)의 압축을 풀고, 디렉토리 이름을 `marmot.server.dist`로 변경한다.
>`$ unzip marmot.server.dist.master.zip`
>`$ mv marmot.server.dist-master marmot.server.dist`

5. `marmot.server.dist/hadoo-conf` 디렉토리로 이동하여 `hadoop-cluster.xml`을 수정한다.
	- 'fs.defaultFS' 속성 값에, HDFS name server의 URL을 기록한다. 안양대 서버를 사용하는 경우는 'hdfs://node00.gsbd.anyang.ac.kr:8020'이다.

<pre><code>
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://node00.gsbd.anyang.ac.kr:8020</value>
</property>
</code></pre>



* GitHub에서 Marmot server 배포판을 download한다.
	- https://github.com/kwlee0220/marmot.server.dist
* Download받은 zip 파일의 압축을 푼다.
	- unzip marmot.server.dist-master.zip
* home 디렉토리 아래에 'marmot'이라는 디렉토리를 생성하고, 압축을 풀어 생성된 디렉토리를 생성된 곳을 이동시킨다.
	이때, 편의상 디렉토리 이름을 marmot.server.dist로 변경시킨다.
	$ cd
	$ mkdir marmot
	$ mv marmot.server.dist-master marmot.server.dist
* '~/marmot/marmot.server.dist' 디렉토리로 이동한다.
	- 'MARMOT_HOME' 환경변수 설정하고, 'PATH' 경로에 추가한다.
		MARMOT_HOME=$HOME/marmot/marmot.server.dist
		PATH=$PATH:$HOME/bin:$MARMOT_HOME/bin

* '~/marmot/marmot.server.dist/bin' 디렉토리로 이동한다.
	$ ln -s marmot-1.1-all.jar marmot.jar

* '~/marmot/marmot.server.dist/hadoo-conf/hadoo-cluster.xml' 파일을 수정한다.
	- 'fs.defaultFS' 속성 값에, HDFS name server의 URL을 기록한다.
		안양대 서버를 사용하는 경우는 'hdfs://node00.gsbd.anyang.ac.kr:8020'이다
		설정 예)
		<property>
			<name>fs.defaultFS</name>
			<value>hdfs://node00.gsbd.anyang.ac.kr:8020</value>
		</property>
	- 'yarn.resourcemanager.address'과 'yarn.resourcemanager.webapp.address' 각각의 속성 값을 설정한다.
		일반적으로 YARN 서버가 운용되는 서버 IP주소에 각각 8050 포트와 8088 포트를 설정한다.
		안양대 서버를 사용하는 경우는 YARN 서버의 IP주소는 node00.gsbd.anyang.ac.kr 이다.
		설정 예)
		<property>
			<name>yarn.resourcemanager.address</name>
			<value>node00.gsbd.anyang.ac.kr:8050</value>
		</property>
		<property>
			<name>yarn.resourcemanager.webapp.address</name>
			<value>node00.gsbd.anyang.ac.kr:8088</value>
		</property>
	- 카다로그 DB 접속을 위한 JDBC 접속정보(JDBC driver URL, 접속 Url, 사용자 이름, 사용자 비밀번호)를 설정한다. 
		안양대 서버를 사용하는 경우는 DB 계정과 DB을 할당받은 후 사용한다.
		설정 예)
		<property>
			<name>marmot.catalog.jdbc.url</name>
			<value>jdbc:postgresql://node00.gsbd.anyang.ac.kr:5432/<계정이름></value>
		</property>
		<property>
			<name>marmot.catalog.jdbc.user</name>
			<value><계정이름></value>
		</property>
		<property>
			<name>marmot.catalog.jdbc.passwd</name>
			<value><계정비밀번호></value>
		</property>
		<property>
			<name>marmot.catalog.jdbc.driver</name>
			<value>org.postgresql.Driver</value>
		</property>





* GitHub에서 Marmot client 배포판을 download한다.
	- https://github.com/kwlee0220/marmot.client.dist
* Download받은 zip 파일의 압축을 푼다.
	- unzip marmot.client.dist-master.zip
* home 디렉토리 아래에 'marmot'이라는 디렉토리를 생성하고, 압축을 풀어 생성된 디렉토리를 생성된 곳을 이동시킨다.
	이때, 편의상 디렉토리 이름을 marmot.client.dist로 변경시킨다.
	$ cd
	$ mkdir marmot
	$ mv marmot.client.dist-master marmot.client.dist
* '~/marmot/marmot.client.dist' 디렉토리로 이동한다.
	- 'MARMOT_CLIENT_HOME' 환경변수 설정하고, 'PATH' 경로에 추가한다.
		MARMOT_CLIENT_HOME=$HOME/marmot/marmot.client.dist
		PATH=$PATH:$MARMOT_CLIENT_HOME/bin
	- 'MARMOT_HOST'와 'MARMOT_PORT' 환경변수를 각각 설정한다.
		MARMOT_HOST는 marmot server가 수행되고 있는 IP 주소로 설정
		MARMOT_PORT는 marmot server가 사용하는 포트 번호로 설정
* '~/marmot/marmot.client.dist/bin' 디렉토리에 있는 여러 script들을 실행시킨다.




