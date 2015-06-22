##CENTOS
#### Install EPEL  
`yum install epel-release`  

##Nagios XI  
Reseting the nagios admin password  
` /usr/local/nagiosxi/scripts/reset_nagiosadmin_password.php --password=<newpassword>`

Fixing the database  
`service mysqld stop`  
`/usr/local/nagiosxi/scripts/repairmysql.sh nagios`  
`service mysqld start`





## /etc/shadow  
Generate a password hash for use in /etc/shadow  
The second field of /etc/shadow contains the password info, it has 3 parts delimited by $.  The first field is a value which denotes the hashing algorithm, ie 6=SHA-512. The key is below.  The second field is the salt, the third is the hash of the password concatenated with the salt.
The default hashing algorithm for Centos/RHEL can be found in /etc/login.def.

1 = MD5  
2 = Blowfish  
2 = eksblowfish  
5 = SHA-256  
6 = SHA-512 

#### Generate a SHA512 password hash, and random salt  

`python -c 'import string; import random; import crypt; salt= "$6$"+"".join(random.choice(string.ascii_uppercase + string.ascii_lowercase + string.digits) for _ in range(8)); print crypt.crypt("password", salt)'`  

$6$JkYSALVJ$fdR5VgjKOv/CTe1N/Go.WqQsw3kfgbsEo2qrBMIgtVGFVIFXceFcJPVlh30h7AHrQajiM6luy7Tg0ikhCS1Fj.



## grep
ignore empty lines or comments  
`grep -v '^\s*\#'`


## PLOS
### Get Random Article
This one pulls a random PLOS article and dumps the text good to use for the -program argument on your screen saver
`curl -s "http://www.plosone.org/article/fetchObject.action?representation=PDF&uri=$(lynx -dump 
www.plosone.org | grep pone\\. | awk '{ print $2; }' | shuf | head -1 | sed -n -e s'/^.*article\///p')" | pdftotext` - -
