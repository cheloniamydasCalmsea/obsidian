
###### swap memory

```js
0. swap 삭제 : sudo swapoff -a
1. sudo fallocate -l 16G /swapfile #16GB 크기의 스왑 파일을 생성합니다.
2. sudo chmod 600 /swapfile #스왑 파일의 권한을 변경하여 보안을 강화합니다.
3. sudo mkswap /swapfile #스왑 파일을 스왑 공간으로 설정합니다.
4. sudo swapon /swapfile #스왑 공간을 활성화합니다.
5. sudo vi /etc/fstab #`/etc/fstab` 파일을 편집하여 부팅 시 스왑파일을 자동으로 활성화합니다.
	* /swapfile swap swap defaults 0 0 #추가
	* sudo echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
1. sudo swapon --show #스왑 공간이 활성화되었는지 확인합니다.
2. free -h
```


###### disk

용량이 잘배분 되어있는지 확인

===/data=== : 첨부파일 및 로그
===/usr/local/service=== : 시스템 및 서비스 포팅




##### ~/.bashrc 설정

~~~python
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# for Careerpass by DainLeaders
#SERVICE BASE
alias sbb='cd /usr/local/service/backend/tomcat'
alias sfb='cd /usr/local/service/frontend/'

#OPERATION SYSTEM
alias sbb.start='/usr/local/service/backend/tomcat/bin/startup.sh'
alias sbb.stop='/usr/local/service/backend/tomcat/bin/shutdown.sh'

alias sbl='cd /uploadData/service_dmz/logs/backend/tomcat'
alias sfl='cd /uploadData/service_dmz/logs/frontend'
alias swl='cd /uploadData/service_dmz/logs/web/nginx'

alias sbl.out='tail -f /uploadData/service_dmz/logs/backend/tomcat/catalina/catalina.out'



# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

*수정후 source ~/.bashrc 로 적용 , 후에 로그인시 자동적용됨*
~~~

