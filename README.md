# FTP Server CentOS 7 Menggunakan vsftpd

## Login SSH

```bash
ssh sandika@192.168.1.16
```

```bash
su -
```

---

## Perbaikan Repository CentOS 7

```bash
sed -i 's/mirror.centos.org/vault.centos.org/g' /etc/yum.repos.d/CentOS-*.repo
```

```bash
sed -i 's/^#.*baseurl=http/baseurl=http/g' /etc/yum.repos.d/CentOS-*.repo
```

```bash
sed -i 's/^mirrorlist=http/#mirrorlist=http/g' /etc/yum.repos.d/CentOS-*.repo
```

```bash
yum clean all
```

```bash
yum makecache
```

---

## Update Sistem

```bash
yum update -y
```

---

## Install vsftpd

```bash
yum install vsftpd -y
```

---

## Menjalankan Service FTP

```bash
systemctl start vsftpd
```

```bash
systemctl enable vsftpd
```

```bash
systemctl status vsftpd
```

---

## Konfigurasi Firewall

```bash
firewall-cmd --permanent --add-port=21/tcp
```

```bash
firewall-cmd --permanent --add-service=ftp
```

```bash
firewall-cmd --permanent --add-port=40000-40100/tcp
```

```bash
firewall-cmd --reload
```

---

## Install Nano

```bash
yum install nano -y
```

---

## Konfigurasi vsftpd

```bash
nano /etc/vsftpd/vsftpd.conf
```

Isi file:

```conf
# Konfigurasi Dasar
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=YES
listen_ipv6=NO

# Konfigurasi Keamanan & Chroot
pam_service_name=vsftpd
userlist_enable=YES
userlist_deny=NO
tcp_wrappers=YES
chroot_local_user=YES
chroot_list_enable=YES
chroot_list_file=/etc/vsftpd/chroot_list
allow_writeable_chroot=YES

# Konfigurasi Direktori & Pasif
local_root=/var/www/html
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```

---

## Membuat User FTP

```bash
useradd -m -s /bin/bash ftpuser
```

```bash
passwd ftpuser
```

---

## Membuat chroot_list

```bash
touch /etc/vsftpd/chroot_list
```

---

## Membuat Direktori FTP

```bash
mkdir -p /var/www/html
```

---

## Mengatur Home Directory FTP User

```bash
usermod -d /var/www/html ftpuser
```

---

## Mengatur Permission

```bash
chown -R ftpuser:ftpuser /var/www/html
```

```bash
chmod -R 755 /var/www/html
```

```bash
chmod -R 777 /var/www/html
```

---

## Membuat File Percobaan

```bash
echo "Halo, ini file percobaan FTP" > /var/www/html/test_ftp.txt
```

---

## Menambahkan User ke user_list

```bash
nano /etc/vsftpd/user_list
```

Tambahkan:

```text
ftpuser
```

---

## Restart Service FTP

```bash
systemctl restart vsftpd
```

---

## Cek Status FTP

```bash
systemctl status vsftpd
```

```bash
ss -ntlp | grep vsftpd
```

---

## Nonaktifkan SELinux

```bash
setenforce 0
```

---

## Melihat Isi Direktori FTP

```bash
ls -la /var/www/html
```

---

## Pengujian FTP dari Windows

```text
ftp://192.168.1.16
```

Login:

```text
Username : ftpuser
Password : rian12345
```

---

## Hasil Upload File FTP

```bash
ls -la /var/www/html
```
