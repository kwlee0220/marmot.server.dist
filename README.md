## Marmot 서버 배포판 설치 방법

### 1. 설치 사전 조건
* Oracle Java가 (Java8 이상) 설치되어 있어야 한다. OpenJDK는 지원하지 않는다.
* Hadoop 2.0 관련 클라이언트 라이브러리가 설치되어 있어야 하고, 사용하려는 hadoop
 	시스템과 네트워크를 통해 연결할 수 있어야 한다. 필요한 최소 Hadoop 기능은
	다음과 같다.
	- HDFS
	- Yarn
	- MapReduce
* HDFS를 사용할 수 있는 사용자 계정이 있다고 가정하고, HDFS 계정이름과 OS 사용자 계정이름이
동일해야 한다.
* DBMS를 사용할 수 있어야 하고, 이를 JDBC를 통해 연결할 수 있어야 한다. 현재 marmot 서버는
PostgreSQL를 활용하여 개발되었기 때문에 가능하면 PostgreSQL를 사용하는 것을 권장한다.
	- DBMS 사용자 계정
	- 데이터베이스 이름

### 2. 설치 절차

Home 디렉토리 아래에 'marmot'이라는 디렉토리를 생성한다.
<pre><code>$ cd
$ mkdir marmot
$ cd marmot
</code></pre>

GitHub에서 Marmot server 배포판을 download한다.
* URL 주소: `https://github.com/kwlee0220/marmot.server.dist`

Download받은 zip 파일 (`marmot.server.dist-master.zip`)의 압축을 `$HOME/marmot` 디렉토리에 풀고,
생성된 디렉토리 이름을 `marmot.server.dist`로 변경한다.
<pre><code>$ unzip marmot.server.dist-master.zip
$ mv marmot.server.dist-master marmot.server.dist
</code></pre>

`hadoop-conf` 디렉토리(전체 경로명: `$HOME/marmot/marmot.server.dist/hadoo-conf`)로 이동하여
`hadoop-cluster.xml`을 수정한다.
* 'fs.defaultFS' 속성 값에, HDFS name server의 URL을 기록한다.
* `yarn.resourcemanager.address`과 `yarn.resourcemanager.webapp.address` 각각의 속성 값을 설정한다.
	일반적으로 YARN 서버가 운용되는 서버 IP주소에 각각 8050 포트와 8088 포트를 설정한다.
* 카다로그 DB 접속을 위한 JDBC 접속정보를 설정한다.
	- `marmot.catalog.jdbc.url`: JDBC 접속 URL (형식: `jdbc:postgresql://<db-server-ip>:<jdbc-port>/<db-name>`)
	- `marmot.catalog.jdbc.user`: 데이터베이스 사용자 이름
	- `marmot.catalog.jdbc.passwd`: 데이터베이스 사용자 패스워드
	- `marmot.catalog.jdbc.driver`: JDBC driver 클래스 이름
		(예를들어 Postgresql인 경우는 `org.postgresql.Driver`)

서버 구동에 필요한 환경변수를 아래와 같이 설정한다.
로그인 때마다 동일한 작업을 반복하지 않기 위해 설정명령을 `.bash_profile` 또는 `.bashrc` 등에 기록할 수 있다.
<pre><code>export MARMOT_HOME=$HOME/marmot/marmot.server.dist
export PATH=$PATH:$MARMOT_HOME/bin
</code></pre>

'$HOME/marmot/marmot.server.dist/bin' 디렉토리로 이동하고, jar 파일에 대한 symbolic link를 생성한다.
<pre><code>$ cd $HOME/marmot/marmot.server.dist/bin
$ ln -s marmot-1.1-all.jar marmot.jar
</code></pre>

데이터베이스 포맷 및 시스템 내부 카다로그 생성한다.
>`$ format_catalog`

원격 접속용 서버 구동시켜서 서버 설정 완료 여부를 확인한다.
> `$ marmot_server`

### 3. 샘플 공간 빅데이터 적재
다음 목록은 Marmot 서버 테스트 및 [marmot.sample](https://github.com/kwlee0220/marmot.sample) 수행을 위해
제공되는 테스트용 공간 데이터 목록이다. 아래 데이터들을 적재하기 위해서는 marmot 서버 뿐만 아니라
[marmot client 배포판](https://github.com/kwlee0220/marmot.client.dist)도 설치되어야 한다.
* [서울시내 지하철 역사](http://gofile.me/2wzSJ/2ODDNCSG8) (출처: 공공데이터포털)
	- 저장위치: $HOME/marmot/data/서울지하철역사
* [전국 주유소 유류 가격](http://gofile.me/2wzSJ/OyrZSdLR6) (출처: 공공데이터포털)
	- 저장위치: $HOME/marmot/data/주유소_가격
* [서울시내 버스 정류소](http://gofile.me/2wzSJ/qyaAYuBiJ) (출처: 도로명 주소)
	- 저장위치: $HOME/marmot/data/정류소
* [전국 법정구역](http://gofile.me/2wzSJ/MJrdNKsNR) (출처: 도로명 주소)
	- 저장위치: $HOME/marmot/data/법정구역_5179
* [전국 건물주소 및 위치](http://gofile.me/2wzSJ/8hoT3yYPb)  (출처: 도로명 주소)
	- 저장위치: $HOME/marmot/data/건물_위치정보

다운로드 받은 샘플 공간 빅데이터를 저장할 디렉토리 `$HOME/marmot/data`를 만들고, 환경변수 `$MARMOT_DATA`에
해당 디렉토리를 설정한다.
> `$ export MARMOT_DATA=$HOME/marmot/data`

Marmot 서버를 시작시킨다.
> `$ marmot_server`

Shapefile이 아닌 일반 텍스트 파일이 저장될 HDFS 파일시스템 내의 디렉토리 `data` 및 관련 하위 디렉토리들을 생성한다.
> `$ hadoop fs -mkdir data`
> `$ hadoop fs -mkdir data/POI`

다운로드 받은 지도 정보를 다음과 같은 과정으로 marmot 서버에 적재시킨다.
* 서울시내 지하철 역사
<pre><code>import_shapefile $MARMOT_DATA/서울지하철역사 -dataset 교통/지하철/서울역사 -charset euc-kr
cluster_dataset 교통/지하철/서울역사</code></pre>
* 전국 주유소 유류 가격
<pre><code>hadoop fs -copyFromLocal $MARMOT_DATA/주유소_가격 data/POI
mc_bind_dataset -type csv data/POI/주유소_가격 -dataset POI/주유소_가격  -geom_col the_geom -srid EPSG:5186
</code></pre>
* 서울시내 버스 정류소
<pre><code>hadoop fs -mkdir data/교통
hadoop fs -mkdir data/교통/버스
hadoop fs -mkdir data/교통/버스/서울
hadoop fs -copyFromLocal $MARMOT_DATA/정류소 data/교통/버스/서울/정류소
mc_bind_dataset -type csv data/교통/버스/서울/정류소 -dataset 교통/버스/서울/정류소 -geom_col the_geom -srid EPSG:5186
</code></pre>
* 전국 법정구역
<pre><code>import_shapefile $MARMOT_DATA/법정구역_5179/시도 -dataset 구역/시도 -charset euc-kr
import_shapefile $MARMOT_DATA/법정구역_5179/시군구 -dataset 구역/시군구 -charset euc-kr
cluster_dataset 구역/시군구
import_shapefile $MARMOT_DATA/법정구역_5179/읍면동 -dataset 구역/읍면동 -charset euc-kr
cluster_dataset 구역/읍면동
import_shapefile $MARMOT_DATA/법정구역_5179/리 -dataset 구역/리 -charset euc-kr

import_shapefile $MARMOT_DATA/법정구역_5179/시도/TL_SCCO_CTPRVN_11.shp -dataset 시연/서울특별시 -charset euc-kr
</code></pre>
* 전국 건물주소 및 위치
<pre><code>hadoop fs -mkdir data/도로명주소
hadoop fs -copyFromLocal $MARMOT_DATA/건물_위치정보 data/도로명주소
mc_bind_dataset -type csv data/도로명주소/건물_위치정보 -dataset 주소/건물POI -geom_col the_geom -srid EPSG:5186
cluster_dataset 주소/건물POI
</code></pre>

Marmot client의 `mc_explorer' 명령을 수행시켜 적재된 데이터를 확인한다.
> `$ mc_explorer`
