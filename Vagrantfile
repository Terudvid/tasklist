Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.2"
  config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.network "private_network", ip: "192.168.50.30"

config.vm.provision "shell", inline: <<-SHELL
 yum -y update
 yum -y install http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
 yum -y install httpd git yum
 yum -y --enablerepo=remi,remi-php71 install php php-cli php-common php-devel php-fpm php-gd php-mbstring php-mysqlnd php-pdo php-pear php-pecl-apcu php-soap php-xml php-xmlrpc php-pecl-zip php-zip php-intl

 php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
 php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
 php composer-setup.php
 php -r "unlink('composer-setup.php');"
 mv composer.phar /usr/local/bin/composer

 yum -y remove mysql*
 yum -y install http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
 yum -y install mysql mysql-devel mysql-server
 #yum install mysql mysql-devel mysql-server mysql-utilities

SHELL

config.vm.provision "shell", run: "always", inline: <<-SHELL
 systemctl restart httpd.service
SHELL

end
