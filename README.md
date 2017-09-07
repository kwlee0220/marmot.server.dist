## Marmot 서버 배포판 설치 방법

### 1. 설치 사전 조건
* Oracle Java가 (Java8 이상) 설치되어 있어야 한다.
* Hadoop 2.0 관련 클라이언트 라이브러리가 설치되어 있어야 하고, 사용하려는 hadoop
 	시스템과 네트워크를 통해 연결할 수 있어야 한다. 필요한 최소 Hadoop 기능은
	다음과 같다.
	- HDFS
	- Yarn
	- MapReduce
* DBMS를 사용할 수 있어야 하고, 이를 JDBC를 통해 연결할 수 있어야 한다.
	- DBMS 사용자 계정
	- 데이터베이스 이름

### 2. 설치 절차
1. Home 디렉토리 아래에 'marmot'이라는 디렉토리를 생성한다.
<pre><code>$ cd
$ mkdir marmot
</code></pre>

2. 압축을 풀어 생성된 디렉토리를 생성된 곳을 이동시킨다.
<pre><code>$ cd marmot</code></pre>

3. GitHub에서 Marmot server 배포판을 download한다.
	* URL 주소: `https://github.com/kwlee0220/marmot.server.dist`

4. Download받은 zip 파일 (`marmot.server.dist-master.zip`)의 압축을 풀고, 디렉토리 이름을 `marmot.server.dist`로 변경한다.
<pre><code>$ unzip marmot.server.dist-master.zip
$ mv marmot.server.dist-master marmot.server.dist
</code></pre>

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

6. 환경변수를 설정한다. 로그인 때마다 동일한 작업을 반복하지 않기 위해 설정명령을
	`.bash_profile` 또는 `.bashrc` 등에 기록할 수 있다.
<pre><code>export MARMOT_HOME=$HOME/marmot/marmot.server.dist
export PATH=$PATH:$HOME/bin:$MARMOT_HOME/bin
</code></pre>

7. '$HOME/marmot/marmot.server.dist/bin' 디렉토리로 이동하고, jar 파일에 대한 symbolic link를 생성한다.
<pre><code>$ cd $HOME/marmot/marmot.server.dist/bin
$ ln -s marmot-1.1-all.jar marmot.jar
</code></pre>


### 3. Marmot 서버 구동

1. 데이터베이스 포맷 및 시스템 내부 카다로그 생성
>`$ format_catalog`

2.서버 구동
> `$ remote_marmot`
