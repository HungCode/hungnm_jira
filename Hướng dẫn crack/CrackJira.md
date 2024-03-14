//-----clean docker----//

Stop the container(s) using the following command:
* docker-compose down

Delete all containers using the following command:
* docker rm -f $(docker ps -a -q)

Delete all volumes using the following command:
* docker volume rm $(docker volume ls -q)

Restart the containers using the following command:
* docker-compose up -d

To delete all containers including its volumes use,

docker rm -vf $(docker ps -aq)
To delete all the images,

docker rmi -f $(docker images -aq)

//-----clean docker----//


-------------------
* Cài đặt postgres
* Cài đặt jira
* Cài đặt confluence
-------------------
* Tạo network chung cho cả service
    * docker network create atlassian

    * port
        * jira: -p 8081:8080
        * confluence: -p 8090:8090 -p 8091:8091
        * postgres: -p 5432:5432 
        * gitlab: 8085
    * Tạo volume: 
        * docker volume create asc-postgres
        * docker volume create jiraVolume
        * docker volume create jiraVolumeOpt
        * docker volume create confluenceVolume
        * docker volume create confluenceVolumeOpt

Cài đặt postgres

* docker run -d --name atlassian-postgres --network atlassian -p 5432:5432 -v atlassian-postgres:/var/lib/postgresql -e 'POSTGRES_USER=asc' -e 'POSTGRES_PASSWORD=Asc@123' -e 'POSTGRES_DB=jira' -e 'POSTGRES_ENCODING=UNICODE' -e 'POSTGRES_COLLATE=C' -e 'POSTGRES_COLLATE_TYPE=C' postgres:14.1

Cài đặt jira

* docker run -d --name asc-jira --network atlassian -v jiraVolume:/var/atlassian/jira -v jiraVolumeOpt:/opt/atlassian/  -p 8081:8080 atlassian/jira-software:8.21.0

    =============Memory / Heap Size=============
    
    If you need to override Confluence Server's default memory allocation, you can control the minimum heap (Xms) and maximum heap (Xmx) via the below environment variables.
    
    JVM_MINIMUM_MEMORY (default: 1024m)
    
    The minimum heap size of the JVM
    
    JVM_MAXIMUM_MEMORY (default: 1024m)
    
    The maximum heap size of the JVM
    
    JVM_RESERVED_CODE_CACHE_SIZE (default: 256m)
    
    The reserved code cache size of the JVM


Cài đặt confluence

* docker run --name  asc-confluence --network atlassian -v confluenceVolume:/var/atlassian/confluence -v confluenceVolumeOpt:/opt/atlassian/ -d -p 8090:8090 -p 8091:8091 atlassian/confluence:7.15

4. Crack jira & confluence
*Crack Jira
    * Tạo thư mực extras\decoder\v2 trong /var/lib/docker/volumes/jiravolumeOpt/_data/jira/atlassian-jira/WEB-INF/classes/com/atlassian/
    * Copy file Version2LicenseDecoder.class đến thư mục vừa tạo /var/lib/docker/volumes/jiravolumeOpt/_data/jira/atlassian-jira/WEB-INF/classes/com/atlassian/extras\decoder\v2
    * Copy file jira-crack\atlassian-extras-3.2.jar đến /var/lib/docker/volumes/jiravolumeOpt/_data/jira/atlassian-jira/WEB-INF/lib/
    * -> Restart service docker
    * Truy cập http://10.10.40.82:8081
    * Decode key: 
    * mail: hungnm@asias.com.vn
    * Sửa nội dung decode từ key genareted liciense:
    
AAAB3Q0ODAoPeNp9kkFzokAQhe/8Cqr2sltbECAuqFVUrRnGSCJoBK0ccplgq5PIwM4ALv9+EbDMR
uKxa7r7ffNef/MSJjsQyYYla71h79dQt2QUhLKhGbr0DuUKuKAJs3VT0yytf3urS1sOwHZJmgJXp
zQCJgCvaXbswn6IF/OFG2DJz+NX4LPNUlQbbEWXUMIyEmU+icHe5WzL4t9EUCLUKInVgklvlBP1Y
mqe82hHBDgkA/sIpeiGYlhSKxyWKdQb0czz8AK5o+npCf9NKS9Pc4ai6YphniiwR+i+EyMAXgB3H
ftu/Gwpy1Vwr1izcKDcjzWvYUx5ss6jTD0Wikg22YFwUKultAA74zlca6t4CAKWAW9a9w3shIid7
SENjZ/QZBw+9Ubu4PD2KIz1BEemn6+S8ueDvnWXG/RnAfN1dNc3Aw+PzPimeH7vl/Hhxn2xpSB/F
RGnaR3GGeXrlDqy7LK0cqtCZoRFX9h65ccXkbY6lcVT1wmwr0x1a6Bppm41az5bVLXYHW3dakFG+
HFyQ/YCpBnfEkYFqb89CpCEONTF53NqYzhdu/GfMTVLyqlo43XgbPJDpS4Hrbr8/cguN/A/XoYyL
sg+rwUb5osjueL5R4KPc+edTf0PdgtJajAsAhRSRzp9f8v4bss1N0QxYXWLGP9DywIUWG01MZKon
6LQXLrXlqeZZh4RqXg=X02mm


*Crack Confluence
    * Tạo thư mực extras\decoder\v2 trong /var/lib/docker/volumes/cfvolumeOpt/_data/confluence/confluence/WEB-INF/classes/com/atlassian/
    * Copy file Version2LicenseDecoder.class đến thư mục vừa tạo /var/lib/docker/volumes/jiravolumeOpt/_data/jira/atlassian-jira/WEB-INF/classes/com/atlassian/extras\decoder\v2
    * Copy file jira-crack\atlassian-extras-3.2.jar đến /var/lib/docker/volumes/cfvolumeOpt/_data/confluence/confluence/WEB-INF/lib/
    * -> Restart service docker   
    * Truy cập http://10.10.40.82:8090
    * Decode key: 
    * mail: hungnm@asias.com.vn
    * Sửa nội dung decode từ key genareted liciense: 
Key liniense trong file

Áp dụng version jira 8.16 và cũ hơn, version 8.21 đã thử và không thành công
Confluence version 7.15.0

Nếu key liciense không dùng được
 Genarate key trial 
 Decode key trial (php  Encode_Decode_liciense.php -d fileCode.txt)
 Sửa expire date & expireLiciense, Evaluation -> false  
 Encode key mới: php  Encode_Decode_liciense.php -e fileKey.txt
 -> New Liciense
 -> Chạy chương trình php  Encode_Decode_liciense.php -e/-d file.txt

example:
  php  Encode_Decode_liciense.php -e key_jira_hungnm_asc.txt
  
-> result <-
#Mon Dec 27 19:59:22 CST 2021
keyVersion=1600708331
ContactName=hungnm@asias.com.vn
conf.active=true
PurchaseDate=2021-12-28
LicenseExpiryDate=2099-01-27 ->> doi thành 99
ContactEMail=hungnm@asias.com.vn
ServerID=BWX7-JZDL-I2QP-FIHT
licenseHash=MCwCFBg7aySxj7MEy90dhcjyBESSweBeAhQrIYv5A5v9UT5pvRIKVsVuMkpgfg\=\=
conf.DataCenter=true
Subscription=true
MaintenanceExpiryDate=2099-01-27 ->> doi 
conf.NumberOfUsers=-1
conf.Starter=false
LicenseID=LIDSEN-L17902301
SEN=SEN-L17902301
Organisation=ASC
CreationDate=2021-12-28
conf.LicenseTypeName=COMMERCIAL
licenseVersion=2
Description=Confluence (Data Center)\: Evaluation
Evaluation=false  


Tạo images backup:
Commit and push new images
* docker commit -m "Crack Jira-8.16.0" b75ba1c09514 phieuphieu/jira-software-8.16.0:test
* docker images
* docker push phieuphieu/jira-software-8.16.0:test


Link truy cập jira: http://10.10.40.82:8081
Acc: admin/admin

Link confluence: http://10.10.40.82:8090
Acc admin/admin

DB postgres: 10.10.40.82:5432
Acc: atlassian/Asc@123

docker commit -m "Init postgres for gitlab, jira, confluence" 5243e2d7f022 phieuphieu/postgres:14.1

docker commit -m "Init jira 8.16" 5243e2d7f022 phieuphieu/jira:


* docker run -d --name asc-postgres --network asc-env -p 5432:5432 -e POSTGRES_PASSWORD=password -v asc-postgres:/var/lib/postgresql phieuphieu/postgres:14.1

docker run -d --name asc-jira --network asc-env -v jiraVolume:/var/atlassian/jira -v jiraVolumeOpt:/opt/atlassian/  -p 8081:8080 atlassian/jira-software:8.21.0

docker run -d --name atlassian-postgres --network atlassian -p 5432:5432 -v atlassian-postgres:/var/lib/postgresql -e 'POSTGRES_USER=asc' -e 'POSTGRES_PASSWORD=Asc@123' -e 'POSTGRES_DB=jira' -e 'POSTGRES_ENCODING=UNICODE' -e 'POSTGRES_COLLATE=C' -e 'POSTGRES_COLLATE_TYPE=C' postgres:14.1


docker run --network asc-env --restart=always --name postgres -d -p 5432:5432 -e 'POSTGRES_USER=asc' -e 'POSTGRES_PASSWORD=Asc@123'   -e 'POSTGRES_DB=jira' -e 'POSTGRES_ENCODING=UNICODE' -e 'POSTGRES_COLLATE=C' -e 'POSTGRES_COLLATE_TYPE=C' postgres:14.1

docker run -d --name postgres  -p 5432:5432 -v atlassian-postgres:/var/lib/postgresql -e POSTGRES_USER=asc -e POSTGRES_PASSWORD=Asc@123 -e POSTGRES_DB=test -e POSTGRES_ENCODING=UNICODE -e POSTGRES_COLLATE=C -e POSTGRES_COLLATE_TYPE=C postgres:14.1