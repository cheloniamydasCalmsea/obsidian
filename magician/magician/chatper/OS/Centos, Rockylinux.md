
###### swap memory

```js
1. sudo fallocate -l 16G /swapfile #16GB 크기의 스왑 파일을 생성합니다.
2. sudo chmod 600 /swapfile #스왑 파일의 권한을 변경하여 보안을 강화합니다.
3. sudo mkswap /swapfile #스왑 파일을 스왑 공간으로 설정합니다.
4. sudo swapon /swapfile #스왑 공간을 활성화합니다.
5. sudo vi /etc/fstab #`/etc/fstab` 파일을 편집하여 부팅 시 스왑파일을 자동으로 활성화합니다.
	* /swapfile swap swap defaults 0 0 #추가
6. sudo swapon --show #스왑 공간이 활성화되었는지 확인합니다.
7. free -h
```


###### disk

용량이 잘배분 되어있는지 확인

===/data=== : 첨부파일 및 로그
===/usr/local/service=== : 시스템 및 서비스 포팅




