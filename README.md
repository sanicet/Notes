# Notes
Git Stages
 Working Directory | Stage | Local Repo | Commit

Working with Local repository

Task #1 initialize a git repository
git init 

Task #2 check the status of git repository
git status

Task #3 add a file to staging area
git add <fileName> 

Task #4 add multiple modified / created to files to staging area
git add .

Task #5 commit a staged change
git commit -m "added first commit"

SSL Certificate commands

To generate an RSA key, use the genrsa command:
openssl genrsa -aes128 -out fd.key 2048
 
To see the information in private key
openssl rsa -text -in private.key

If you need to have just the public part of a key separately, you can do that with the following rsa command:
$ openssl rsa -in fd.key -pubout -out fd-public.key

Generate a CSR with a new Private Key
openssl req -out <CSR>.csr -new -newkey rsa:2048 -nodes -keyout <unencrypted name>.key
 
Generate a CSR with an existing private Key
openssl req -new -key private.key -out fd.csr

check that the CSR is correct
openssl req -text -in fd.csr -noout

Creating CSRs from Existing Certificates
openssl x509 -x509toreq -in fd.crt -out fd.csr -signkey fd.key

Signing a CSR to generate self-signed certificate
openssl x509 -req -days 365 -in fd.csr -signkey fd.key -out fd.crt

Examining certificate
openssl x509 -text -in fd.crt -noout

PKCS#12- A complex format that can store and protect a server key along with an entire certificate chain. It’s commonly seen with .p12 and .pfx extensions.

convert the key and certificates in PEM format to PKCS#12
openssl pkcs12 -export \
    -name "My Certificate" \
    -out fd.p12 \
    -inkey fd.key \
    -in fd.crt \
    -certfile fd-chain.crt
Enter Export Password: ****************
Verifying - Enter Export Password: ****************

command for  PKSC#12 to extract detail

openssl pkcs12 -in fd.p12 -out fd.pem -nodes

WGET to download all the files in a webpage
wget --http-user= --http-password= -r -np -k <<url>>

Security notes
https://cwe.mitre.org/data/definitions/693.html

Microservices 

Rest best practices
https://medium.com/@dilankam/restful-api-design-best-practices-principles-ded471f573f3

folder structure best practices

https://medium.com/the-resonant-web/spring-boot-2-0-project-structure-and-best-practices-part-2-7137bdcba7d3

config
controller
dto
exception
model
repository
security
service
util


Linux
 Find 
   - https://www.suse.com/c/linux-command-line-i/
   eg ; find /etc -type f | xargs grep -l -i "damian"


 to increase the time out:

 Resolution
   Edit the /etc/ssh/sshd_config file
   Un-remark or add the ClientAliveInterval parameter and set it to the number of seconds (of inacitivty) that ssh should wait before     terminating the connection.
   Un-remark or add the ClientAliveCountMax and set it to 0.
   Restart the ssh daemon.
   Additional Information
   An example configuration would be:

   ClientAliveInterval 600
   ClientAliveCountMax 0

   This would clear any ssh sessions that have been inactive for 10 minutes.

 RPM
   https://www.suse.com/c/useful-rpm-commands/
   
  SQL :
  
Getting size of the table
  
select owner, segment_name, sum(bytes)/1024/1024 MB from dba_segments where segment_name in ('table name');

select owner, segment_name, sum(bytes)/1024/1024  MB from dba_segments where segment_name in (select index_name from dba_indexes where table_name='table name') 
and owner='schema name';

Coverting Blob to clob
select COUNT(*)
  from table 
 where dbms_lob.instr (EVENT_DATA, -- the blob
                   utl_raw.cast_to_raw ('ggg'), -- the search string cast to raw
                   1, -- where to start. i.e. offset
                   1 -- Which occurrance i.e. 1=first
                    ) > 0 -- location of occurrence. Here I don't care.  Just
and CREATE_DT>SYSDATE-1
                    
                    ;


Spring:
I prefer using constructor injection (over field and setter injection) to keep the state in my components immutable. The immutable state is important if you want to be able to run the component in a multithreaded runtime environment.




