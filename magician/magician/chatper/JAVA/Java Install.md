
```js
dnf list java* #java 버전 확인 인후 install 진행
yum list java* #java 버전 확인 인후 install 진행

dnf install 패키지명
yum install 패키지명
```

*yum의 단점을 보완해서 나온게 dnf라고 이해하면 좋다.*


##### 수동 설치

##### rpm
```js
https://www.oracle.com/kr/java/technologies/downloads/#java21
에서 .rpm 확장자 다운로드

rpm -Uvh java파일.rpm

java -version
javac -verison 

```

##### tar.gz
```js
https://www.oracle.com/kr/java/technologies/downloads/#java21
에서 tar.gz 확장자 다운로드

압축푼 후에 JAVA_HOME을 따로 지정 필요

# ~/.bash_profile
  /etc/profile
...

source /config파일 #변경사항 적용, 매번 source ..를 수행할수없기에 config file에 선언

java -version
javac -verison 

```