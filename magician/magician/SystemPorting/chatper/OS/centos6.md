~~~
echo "http://vault.centos.org/6.10/sclo/x86_64/sclo" > /var/cache/yum/x86_64/6/base/mirrorlist.txt
echo "http://vault.centos.org/6.10/sclo/x86_64/sclo" > /var/cache/yum/x86_64/6/extras/mirrorlist.txt
echo "http://vault.centos.org/6.10/sclo/x86_64/sclo" > /var/cache/yum/x86_64/6/updates/mirrorlist.txt
wget https://vault.centos.org/6.6/os/x86_64/Packages/libcgroup-0.40.rc1-12.el6.x86_64.rpm
rpm -ivhf libcgroup-0.40.rc1-12.el6.x86_64.rpm
wget https://get.docker.com/rpm/1.7.1/centos-6/RPMS/x86_64/docker-engine-1.7.1-1.el6.x86_64.rpm
rpm -ivhf docker-engine-1.7.1-1.el6.x86_64.rpm
service docker start
chkconfig docker on
~~~

