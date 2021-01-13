Шаблон создан Евгением Овчинниковым для ознакомительных целей.
Совместимость не гарантируется и поддержка тоже.

шаблон проверен на distributed режиме minio, в режиме федерации или кубере не проверялось.


1. Для работы нужно выполнить на хосте ряд действий:

sudo cp zabbix_minio_template /etc/sudoers.d/zabbix_minio_template
sudo cp minio.conf /etc/zabbix/zabbix.agent.conf.d/minio.conf
sudo service zabbix-agent restart

HOST=minio01.domain.com
MINIOLOGIN='type_your_login'
MINIOPASSWORD='type_your_password'

sudo wget https://dl.min.io/client/mc/release/linux-amd64/mc -O /usr/bin/mc
sudo chmod +x /usr/bin/mc
sudo chown zabbix /usr/bin/mc

sudo mkdir -p /var/lib/zabbix/.mc/certs/CAs
sudo chown zabbix -R /var/lib/zabbix/
sudo -H -u zabbix /usr/bin/mc config host add local https://${HOST}:9000 ${MINIOLOGIN} ${MINIOPASSWORD}
sudo -H -u zabbix /usr/bin/mc admin info local

2. На Zabbix-server импортируем шаблон и применяем его к одному из хостов кластера.
