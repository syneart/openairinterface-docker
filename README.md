# openairinterface-docker

Docker container with N210/B210/B200-mini

Requirements

Docker 1.9.1 (devicemapper)
====================================
---------------------------------------
    # docker info
Server Version: 1.9.1<br>
Storage Driver: devicemapper <br>
Root Dir: /var/lib/docker/devicemapper <br>
　Backing Filesystem: extfs<br>
　Dirs: 31<br>
　Dirperm1 Supported: true<br>
Execution Driver: native-0.2<br>
Logging Driver: json-file<br>
Kernel Version: 3.19.0-19-generic<br>
Operating System: Ubuntu 14.04<br>
CPUs: 1<br>

---------------------------------------
Ubuntu 14.04.03 LTS (or later) [3.19 kernel] <br>
gnuradio-3.7.8.1 (or later)<br>
UHD-3.9.0 (or later)<br>

---------------------------------------

[eNB] <br>
commit 0c74ab15f4cb15f001c7e2d0acd371f5fb1099d1 <br>
Merge: 2be7706 7f4ec78 <br>
Author: Rohit Gupta <rohit.gupta@eurecom.fr> <br>
Date:   Sat Jun 18 22:17:09 2016 +0200 <br>
　　Merge branch 'feature-34-test_framework' <br><br>
[EPC] <br>
commit 6edc4e836ccda3098a2c6d2969a3b84faee91362

---------------------------------------
=first of all you need to have admin privileges  
    
    $ sudo su -
    
=setup Docker

    Uninstall old versions    
    $ sudo apt-get remove docker docker-engine docker.io

Extra packages for Trusty 14.04
    
    $ sudo apt-get update
    $ sudo apt-get install \
        linux-image-extra-$(uname -r) \
        linux-image-extra-virtual -y

Install using the repository
    
    $ sudo apt-get update
    $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common -y
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

x86_64:
    
    $ sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"
Install Docker CE
    
    $ sudo apt-get update
    $ sudo apt-get install docker-ce -y
    $ sudo su
    # gedit /etc/default/docker

加入DOCKER_OPTS="--storage-driver=devicemapper" <br>
重開機並開啟terminal使用docker info查看檔案格式是否改為devicemapper (預設為aufs)

\<Enter Running Container>

=Give terimal some color
    
    # sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/g' '/root/.bashrc'

=Install PyBOMBS
    
    # apt-get install python-pip
    # pip install -U pip
    # pip install PyBOMBS
    # pybombs prefix init /usr/local -a usrlocal
    # pybombs config default_prefix /usr/local
    # pybombs recipes add gr-recipes git+https://github.com/gnuradio/gr-recipes.git
    # pybombs recipes add gr-etcetera git+https://github.com/gnuradio/gr-etcetera.git
    # pybombs -v install uhd gnuradio
    # ldconfig

=Download uhd images
    
    # /usr/local/lib/uhd/utils/uhd_images_downloader.py

=Test
    
    # uhd_usrp_probe

= First ifconfig

    # ifconfig eth0:11 192.171.11.2
    # ifconfig eth0:21 192.171.21.2

ENB (commit hash: 0c74ab15f4cb15f001c7e2d0acd371f5fb1099d1)
    
    # docker run -i -t --name="oai_enb_new" -h labuser --privileged -v /dev:/dev  -v /proc:/proc ubuntu:14.04 /bin/bash
 	 	 	
    # apt-get update; apt-get upgrade -y
    # apt-get install vim git wget software-properties-common -y
    # dpkg-reconfigure tzdata
    # echo -n | openssl s_client -showcerts -connect gitlab.eurecom.fr:443 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' >> /etc/ssl/certs/ca-certificates.crt
    # git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git	 	 	 	
    # cd /openairinterface5g
    # git checkout -f 0c74ab15f4cb15f001c7e2d0acd371ffb1099d1
    # source oaienv

    # ./cmake_targets/build_oai -I -g --eNB -x --install-system-files -w USRP --install-optional-packages

(hotfix for missing boost_system library) <br>
https://gitlab.eurecom.fr/oai/openairinterface5g/commit/65dedc4cd82b6b20ba10aa51a103442f71c84743)

    # ./cmake_targets/build_oai -w USRP -x -c --eNB
YES

    # cp /openairinterface5g/targets/PROJECTS/GENERIC-LTE-EPC/CONF/enb.band7.tm1.usrpb210.conf /
    # vim /enb.band7.tm1.usrpb210.conf
    
input eth0 ip
#  
run EPC (commit hash: 6edc4e836ccda3098a2c6d2969a3b84faee91362)	 	

    # docker run -i -t  --name="oai_epc_new" -h labuser -p 1111:80 --privileged ubuntu:14.04 /bin/bash

    # apt-get update; apt-get upgrade -y
    # apt-get install vim git wget -y
    # apt-get install linux-image-extra-$(uname -r) -y
OK YES

    # dpkg-reconfigure tzdata
    # echo -n | openssl s_client -showcerts -connect gitlab.eurecom.fr:443 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' >> /etc/ssl/certs/ca-certificates.crt
    # git clone https://gitlab.eurecom.fr/oai/openair-cn.git
    # cd openair-cn
    # git checkout -f 6edc4e836ccda3098a2c6d2969a3b84faee91362
    # cd /openair-cn/SCRIPTS
    # source oaienv


HSS:

    # ./build_hss -i 
    (password: linux)

<HSS configuration> <br>
Content of /usr/local/etc/oai/hss.conf:<br>
	HSS :<br>
	{<br>
	## MySQL mandatory options<br>
	MYSQL_server = "127.0.0.1";<br>
	MYSQL_user= "root";<br>
	MYSQL_pass= "linux";<br>
	MYSQL_db= "oai_db";<br>
	## HSS options<br>
	OPERATOR_key = "1006020f0a478bf6b699f15c062e42b3"; # OP key for oai_db.sql<br>
	RANDOM = "true";<br>
	## Freediameter options<br>
	FD_conf = "/usr/local/etc/oai/freeDiameter/hss_fd.conf";<br>
	};<br>

/etc/hosts file:<br>
For example on a host with hostname 'labuser':

	labuser@labuser:# cat /etc/hosts
	127.0.0.1 localhost
	127.0.1.1 labuser.openair4G.eur labuser  #Your hostname here
	127.0.1.1 hss.openair4G.eur hss
<br>
=check certificate

    # ./check_hss_s6a_certificate /usr/local/etc/oai/freeDiameter hss.openair4G.eur
    # ./build_hss --clean --debug

    # service mysql start ; service apache2 start
    # wget https://www.adminer.org/static/download/4.2.5/adminer-4.2.5-en.php -O /var/www/html/adminer.php
    # mysql -u root --password=linux -e "create database oai_db"
    # mysql -u root --password=linux oai_db < $OPENAIRCN_DIR/SRC/OAI_HSS/db/oai_db.sql
    # cp /etc/hosts ~/oai_hosts
    # echo export OPENAIRCN_DIR=/openair-cn >>  ~/.bashrc
    # echo cp ~/oai_hosts /etc/hosts  >>  ~/.bashrc
    # echo service mysql start  >>  ~/.bashrc
    # echo service apache2 start  >>  ~/.bashrc

MME:

    # ./build_mme -i
    # cp $OPENAIRCN_DIR/ETC/mme.conf /usr/local/etc/oai
    # vim /usr/local/etc/oai/mme.conf
input eth0 ip

    # cp $OPENAIRCN_DIR/ETC/mme_fd.conf /usr/local/etc/oai/freeDiameter
    # vim /usr/local/etc/oai/freeDiameter/mme_fd.conf
change ‘yang’ to ‘labuser’ => Identity = "labuser.openair4G.eur";

    # ./build_mme --clean
    # ./check_mme_s6a_certificate /usr/local/etc/oai/freeDiameter labuser.openair4G.eur
    # ./run_mme

SP-GW:

    # ./build_spgw -i
    # cp $OPENAIRCN_DIR/ETC/spgw.conf /usr/local/etc/oai
    
input eth0 ip & dns

    # ./build_spgw --clean
    # ./run_spgw -r

Install run

    # echo #!/bin/bash >> ~/run_epc
    # echo $OPENAIRCN_DIR/SCRIPTS/run_mme & >> ~/run_epc
    # echo $OPENAIRCN_DIR/SCRIPTS/run_spgw -r >> ~/run_epc
    # echo “alias run_epc='bash ~/run_epc'” >>  ~/.bashrc
