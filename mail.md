## IMAPS  
Testing IMAPS with openssl  
`openssl s_client -connect 192.168.3.116:993`  
`a login user pass`  
List folders  
`a list """*"`  
Select a Folder  
`a select INBOX` 
Fetch the first 5 messages  
`a fetch 1:5 (uid) 

## POP3S  
login, list, retrieve  
`openssl s_client -connect 192.168.3.116:995`  
`user user@domain.com`  
`pass yourpassword`  
`list`  
`retr 1`  
