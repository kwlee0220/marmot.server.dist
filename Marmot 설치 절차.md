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
	- 'fs.defaultFS' 속성 값에, HDFS name server의 URL을 기록한다. 안양대 서버를 사용하는
	경우는 'hdfs://node00.gsbd.anyang.ac.kr:8020'이다. 
	- `yarn.resourcemanager.address`과 `yarn.resourcemanager.webapp.address` 각각의 속성 값을 설정한다.
		일반적으로 YARN 서버가 운용되는 서버 IP주소에 각각 8050 포트와 8088 포트를 설정한다.
		안양대 서버를 사용하는 경우는 YARN 서버의 IP주소는 node00.gsbd.anyang.ac.kr 이다.
	- 카다로그 DB 접속을 위한 JDBC 접속정보를 설정한다.
		* `marmot.catalog.jdbc.url`: JDBC 접속 URL.
			(예: `jdbc:postgresql://node00.gsbd.anyang.ac.kr:5432/<DB 이름>`)
		*  `marmot.catalog.jdbc.user`: 데이터베이스 사용자 이름
		*  `marmot.catalog.jdbc.passwd`: 데이터베이스 사용자 패스워드
		*  `marmot.catalog.jdbc.driver`: JDBC driver 클래스 이름
			(예를들어 Postgresql인 경우는 `org.postgresql.Driver`)

6. 환경변수를 설정한다.
	> `export MARMOT_HOME=$HOME/marmot/marmot.server.dist`
	> `export PATH=$PATH:$HOME/bin:$MARMOT_HOME/bin`

7. '$HOME/marmot/marmot.server.dist/bin' 디렉토리로 이동하고, jar 파일에 대한 symbolic link를 생성한다.
	>`$ cd $HOME/marmot/marmot.server.dist/bin`
	>`$ ln -s marmot-1.1-all.jar marmot.jar`






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




