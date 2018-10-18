sudo apt update
sudo apt upgrade
reboot
sudo apt update
sudo apt install git

sudo apt install isc-dhcp-server

ip a

sudo nano /etc/default/isc-dhcp-server

---------
INTERFACESv4="enp0s25"
---------


sudo nano /etc/dhcp/dhcpd.conf

---------
default-lease-time 600;
max-lease-time 7200;

ddns-update-style none;

authoritative;

subnet 192.168.0.0 netmask 255.255.255.0 {
        range 192.168.0.150 192.168.0.170;
        option subnet-mask 255.255.255.0;
        option routers 192.168.0.1;
        option broadcast-address 192.168.0.255;
        default-lease-time 600;
        max-lease-time 7200;
}

---------

set static ip for enp0s25 to 192.168.0.1

restart computer

tail -f /var/lib/dhcp/dhcpd.leases

go to ip adress in web browser
user: admin
user: admin

you will need internet for the other portion of the script to work.  to do this i will share the wifi with the ethernet.  this will not work without this step.  the script expects to find a tar file on a server.  If it does not it will hang.

check make available to other users the wifi connection and ethernet connection


git clone https://github.com/khufkens/phenocam-installation-tool.git
cd phenocam-installation-tool

./PIT.sh 192.168.0.150 admin admin PHENOCAM_TEST +2 UTC 4 2 15 passive



sudo apt-get install vsftpd
sudo systemctl start vsftpd
sudo systemctl enable vsftpd

sudo ufw allow OpenSSH
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw allow 40000:50000/tcp
sudo ufw allow 990/tcp
sudo ufw enable

sudo mkdir /home/phenocam/ftp
sudo chown root.nogroup /home/phenocam/ftp
sudo chmod 755 /home/phenocam/ftp
sudo mkdir -p /home/phenocam/ftp/data/PHENOCAM_TEST
sudo chmod 777 /home/phenocam/ftp/data/PHENOCAM_TEST
sudo chmod g+rwx /home/phenocam/ftp/data/PHENOCAM_TEST

sudo nano /etc/vsftpd.conf
------------------
listen=NO
listen_ipv6=YES
anonymous_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
anon_upload_enable=YES
anon_other_write_enable=YES
anon_mkdir_write_enable=YES
chown_uploads=YES
chown_username=phenocam
xferlog_enable=YES
xferlog_file=/var/log/vsftp.log
xferlog_std_format=YES
idle_session_timeout=600
data_connection_timeout=120
anon_root=/home/phenocam/ftp/
no_anon_password=YES
pasv_min_port=40000
pasv_max_port=50000
------------------

change the server.txt file on the camera for the ip address of the ftp server'

sudo systemctl restart vsftpd





