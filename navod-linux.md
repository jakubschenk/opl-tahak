# Linux

Notion: [https://linux-skripta.notion.site/Opera-n-syst-m-Linux-ad14d04ffb0143839fcf6aab1c5ea7fd?p=05ef1230cbb94ad7a8c723e38cebc2a8&pm=s]

Nezapomeň povolit vnitřní síť pro enp0s8 před spuštěním stroje! 

`hostnamectl set-hostname _konkretnijmeno_`

kontrola:  

`hostname`

## DEBIAN DHCP SERVER

`nano /etc/network/interfaces`

![Obsah obrázku text, snímek obrazovky, Písmo
Popis byl vytvořen automaticky](images/image-cea1210d-34e9-4927-8b3f-a0a46df4e3ec.png)

`dns-nameservers 8.8.8.8`

`systemctl restart networking`

`apt update `

`apt install isc-dhcp-server -y`

(asi nebude fungovat ale bez toho se tam neobjeví ty dhcpd.conf soubory) 

`nano /etc/dhcp/dhcpd.conf`

![Obsah obrázku text, snímek obrazovky, Písmo
Popis byl vytvořen automaticky](images/image-dbff71a4-2277-4c29-a1a6-8a1478090582.png)

…….

![Obsah obrázku text, snímek obrazovky, Písmo
Popis byl vytvořen automaticky](images/image-88d8e2e1-ad4a-4f10-9b0d-4be020ed1740.png)

![A screen shot of a computer
Description automatically generated](images/image-43a364c5-1de1-4528-833f-646ca6343965.png)

`nano /etc/dhcp/dhcpd6.conf`

![Obsah obrázku text, elektronika, snímek obrazovky, displej
Popis byl vytvořen automaticky](images/image-26db8ccc-bd8b-4c4e-b0b0-6257c8b38e24.png)

…

Zakomentovat!!!!

![Obsah obrázku text, snímek obrazovky, Písmo
Popis byl vytvořen automaticky](images/image-84a482ce-2eb8-4aff-8005-ea5084babe52.png)

…..

![Obsah obrázku text, snímek obrazovky, Písmo, design
Popis byl vytvořen automaticky](images/image-1b502ab6-db41-406e-aa70-6c10d545cc1a.png)

![A black screen with white text
Description automatically generated](images/image-e45e58fa-5137-4000-a3ad-47b09ca1d609.png)

`nano /etc/default/isc-dhcp-server`

![Obsah obrázku text, snímek obrazovky, software, Písmo
Popis byl vytvořen automaticky](images/image-5eee5e64-f22a-4f98-bc84-168e5acc3d9f.png)

`systemctl restart isc-dhcp-server.service`

## UBUNTU – DHCP client

`nano /etc/network/interfaces`

![Obsah obrázku text, snímek obrazovky, Písmo, číslo
Popis byl vytvořen automaticky](images/image-9f479184-2b07-413f-85ec-58cfbeeeb50e.png)

`dhclient -v enp0s8`

možná bude potřeba ještě:

`systemctl restart networking`

Kontrola: 

`ip addr show `

## DEBIAN SSH SERVER

`apt install openssh-server -y`

kontrola jestli ssh běží

`systemctl status ssh`

jestli ne tak:

`systemctl start ssh`

`systemctl enable ssh`

`nano /etc/ssh/sshd_config`

najdi toto:

![Obsah obrázku text, Písmo, snímek obrazovky, Grafika
Popis byl vytvořen automaticky](images/image-d1238035-3681-4296-9adc-c549d64730ff.png)

A změň to na:

`PermitRootLogin yes`

![Obsah obrázku text, Písmo, snímek obrazovky, design
Popis byl vytvořen automaticky](images/image-b420d1d7-9682-4fda-bf74-7a3339380338.png)

Ctrl+o (uložit)

Ctrl+x (zavřít)

Restartuj službu:

`systemctl restart ssh`

## SSH KLIENT UBUNTU

`ssh-keygen -t rsa -b 4096`

a 2x ENTER (ne heslo!!!!)

`ssh-copy-id root@10.0.1.1`

`ssh-copy-id 10.0.1.1` (ansible)

(za zavináčem je ip adresa debianu)

Kontrola jestli se ssh pridal:

`ssh root@10.0.1.1`

jestli se přihlásí bez hesla, je to ok.

Celkově by to na ubuntu mělo vypadat takto:

![Obsah obrázku text, snímek obrazovky, design
Popis byl vytvořen automaticky](images/image-6dbbf960-43a1-4b06-a87d-eab5afc7406b.png)

Po tomto se z ubuntu lze přihlásit pomocí příkazu:

`ssh root@10.0.1.1`

## ANSIBLE

`sudo apt install ansible sshpass`

`sudo mkdir /etc/ansible`

`sudo nano /etc/ansible/hosts`

```
[servers]  
10.0.1.1 ansible_user=student  
10.0.1.2 ansible_user=student
```

![A black and grey rectangular object
Description automatically generated](images/image-71f91bc1-ba46-462c-a1ad-045dd57027e8.png)

`chmod +x ping.sh`

`./ping.sh` -> mělo by jít pokud je zkopirovane ssh id

https://github.com/jakubschenk/opl-tahak

`ansible-playbook tftp_server.yml -K`

`ansible-playbook tftp_client.yml -K`

## APACHE

`apt install -y apache2`

`systemctl status apache2`

`mkdir -p /home/web`

`chown www-data:www-data /home/web`

`chmod 755 /home/web`

![A black screen with a white border
Description automatically generated](images/image-6bda9ba5-e498-45ee-84e1-a1db074e1bb1.png)

`chown www-data:www-data /home/web/index.html`

`chmod 644 /home/web/index.html`

![A screenshot of a computer
Description automatically generated](images/image-644a90c2-cdea-4feb-8a51-a4c9ab3644c4.png)

`a2dissite 000-default.conf`

`a2ensite myweb.conf`

`systemctl reload apache2`

Check v prohlizeci ze funguje http://10.0.1.2

`apt install -y openssl ssl-cert`

`a2enmod ssl`

`mkdir -p /etc/ssl/private`

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 \\ -keyout /etc/ssl/private/www.test2025.com.key \\ -out /etc/ssl/certs/www.test2025.com.crt`

`apt install -y libapache2-mod-security2`

`a2enmod security2`

`systemctl restart apache2`

https://www.linode.com/docs/guides/securing-apache2-with-modsecurity/

test2025-ssl.conf

test2025.com  
  
`nano /etc/hosts` na ubuntu!!!  
`10.0.1.2 www.test2025.com`