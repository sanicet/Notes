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

//Git Clean untracked files and folders
 git clean -fd
 
 //Git reset files which is changed.
 
 Git checkout Head <<file> / no options

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

creating a certificate chain pem
https://www.digicert.com/kb/ssl-support/pem-ssl-creation.htm

command for  PKSC#12 to extract detail

openssl pkcs12 -in fd.p12 -out fd.pem -nodes

SSL handshake info
Section 7.3 https://tools.ietf.org/html/rfc5246
SSL logs debug :https://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html

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

JPA
https://jcp.org/aboutJava/communityprocess/mrel/jsr338/index.html 


Spring:
I prefer using constructor injection (over field and setter injection) to keep the state in my components immutable. The immutable state is important if you want to be able to run the component in a multithreaded runtime environment.

Prefer having the instance variable declared as final if it need to not be changed

Annotations : @SpringBootApplication, @Component, @Bean (used in java configuration), @Configuration, @AutoWired ( injecting a component to another instance variable, @ComponentScan({"package"}), @RestController , @GetMapping(value="/my-resource", produces="application/json"), @PostMapping(value="",produces="")

Spring Webflux : Supports annotation based style or functional style (based on routers and handlers). Instead of RestTemplate, it uses WebClient for http request, It supports servlet based containers but need servlet 3.1 or higher. Or we will have to use embodded non servlet based containers eg Netty

Spring boot property names : application.properties or application.yml
  Change the port eg : server.port:8081


SpringFox is used to document the service based and is based on Swagger /open API specification. it does this by examining tha application while startup to create documents based on inspecting various annnotations eg: webflux, swagger etc


Spring Data - Based on entities and repositories , It supports relational( JPA based) and non relational ( eg : Mongo - document, key-value - Redis, Graph -Neo4j). Annotations - @Entity, @IDClass("idclass.class"), @Table("tableName"), @ID, @Version
Entity - Data to be stored;
Repository -interface used to store and retrieve data from tables in a generic way.
eg :
public interface ReviewRepository extends CrudRepository<ReviewEntity, ReviewEntityPK> {
    Collection<ReviewEntity> findByProductId(int productId);
}
Spring Data will implement the method if naming convention is followed properly.
 
For reactive, use ReactiveCrudRepository - returns Mono(0-1) or Flux(0-m) streams as they are available. Spring data JPA does not support this but spring data MongoDB supports this.

TShark/WireShark
https://blogs.oracle.com/meena/using-tshark-to-debug-ssl-connections
zypper install wireshark
tshark -Y http
tshark -o "ssl.keys_list:csiws2-test.csidentity.com,443,http,shars.pem" > log01.txt
tshark -f "tcp port 443 and host xxx.com or host a.b.c.d" -i eth0 -x -P
tshark -f "tcp port 443 and host xxx.com "  -i  eth0 -V

Curl
Without proxy
curl -H "Accept: application/xml" -H "Content-Type: application/xml" -X POST -k <<url>> -d "<<request>>"
 
with proxy
 curl -x https://172.18.100.15:18717  -H "Accept: application/xml" -H "Content-Type: application/xml" -X POST -k <<url>> -d "<<request>>"
 
With proxy and client certificate
curl -x https://172.18.100.15:18717 --cert-type P12 --cert /export/home/xxx/xxx_seckey-new.p12:password --header "Content-Type: text/xml;charset=UTF-8" --header "SOAPAction: getLockStatus" --data @/export/home/sxxx/Request.xml <<url>> -v

Oracle

Check if a list is not there ::::

with id_list as (
  select 'CA562508150' id from dual union all
  select '1-421J8O' id from dual union all
  select '1-42BF7O' id from dual union all
  select '1-42CVWP' id from dual union all
  select '1-9KSHF9' id from dual
)
  select * from id_list WHERE not EXISTS (
  SELECT reference FROM (SELECT CONSUMER_REF AS reference,CVV_RESPONSE_CD FROM CREDIT_CARD_AUTH_LOG WHERE 
  create_dt BETWEEN TO_DATE('05-20-2020 01:00:00', 'mm-dd-yyyy hh24:mi:ss') AND TO_DATE('05-20-2020 13:00:00', 'mm-dd-yyyy hh24:mi:ss')
  ) a
  WHERE reference=id
  )
  
  
  Select duplicate row with latest date :::::
  
  
  WITH table1 AS (
	SELECT
		AR.CLIENT_CONSUMER_IDENTIFIER,
		EA.EID_AUTHENTICATION_ID,
		EA.TRANSACTION_STATUS,
		EA.ASSESSMENT_PASSED,
		EA.OVERALL_AUTH,
		EA.CREATE_DT_TS
	FROM
		EID_AUTHENTICATION_EVENT EA,
		AUTHENTICATION_REQUEST AR
	WHERE
		EA.EID_AUTHENTICATION_ID = AR.AUTHENTICATION_EVENT_ID
		AND ea.AUTHENTICATION_TYPE_ID = AR.AUTHENTICATION_TYPE_ID
		AND EA.CONSUMER_PARTNER_GRP_ID IN (
			SELECT
				CONSUMER_PARTNER_GROUP_ID
			FROM
				CONSUMER_PARTNER_GROUP
			WHERE
				CONSUMER_PARTNER_GROUP_CD IN (
					'EFX_CA',
					'EFXCA_PTNR'
				)
		)
		AND EA.TRANSACTION_STATUS IN (
			'A',
			'P',
			'F',
			'V',
			'S',
			'X'
		)
		AND EA.CREATE_DT BETWEEN TO_DATE( '05-26-2020 00:00:00', 'mm-dd-yyyy hh24:mi:ss' ) AND TO_DATE( '05-27-2020 23:59:59', 'mm-dd-yyyy hh24:mi:ss' )
		AND AR.CLIENT_CONSUMER_IDENTIFIER IN (
			SELECT
				CLIENT_CONSUMER_IDNT
			FROM
				CONSUMER
			WHERE
				CREATE_DT BETWEEN TO_DATE( '05-26-2020 00:00:00', 'mm-dd-yyyy hh24:mi:ss' ) AND TO_DATE( '05-27-2020 23:59:59', 'mm-dd-yyyy hh24:mi:ss' )
		)
) SELECT
	a.*
FROM
	table1 a
INNER JOIN (
		SELECT
			CLIENT_CONSUMER_IDENTIFIER,
			MAX( CREATE_DT_TS ) AS CREATE_DT_TS
		FROM
			table1
		GROUP BY
			CLIENT_CONSUMER_IDENTIFIER
	) table2 ON
	a.CLIENT_CONSUMER_IDENTIFIER = table2.CLIENT_CONSUMER_IDENTIFIER
	AND a.CREATE_DT_TS = table2.CREATE_DT_TS 
  
Reverse proxy and redirect notes:

https://www.akadia.com/services/apache_redirect.html
  
 # Angular
 #Install angular CLI
 npm -g @angular/cli
 #Create a project
 ng new inspire-attingal-lite-app
 #Create a server and deploy the above app
 ng serve
 #Add bootstrap
 npm install --save bootstrap
 #Generate a component
 ng generate component servers
 #Data binding
 String interpolation (data should return a string, it can be a method, one line typesecript code or reference to a variable in typescript component, or actual string itself - {{data}}. This is normally used in displaying text.
#HTML property Binding
[property]="data" - here property is an html attribute of a tag. data is a property in typescript class. this is normally used to change the behaviour of html attributes ( eg disabled in a button).
 #Event binding
 (event)="function" - here event is an JS event like onClick, function is a method to execute in your typesrcipt class.
 #Event Data binding - how to bind event data to TypeStript function
 (event)="function($event) in Html
 and in type script, have a function(event:Event) or function(event:any). To inspect event in console for debugging, add console.log(event) in type script function.
 
 #Two way data binding
 Two way databinding allows data to be bind from UI to TypeScript and back. For this use [(ngModel)]="buttonName" to tags. ButtonName is property used for binding two way.
 
 #Directives - direcrtives are instructions to DOM. Component is a form of directive which has template url. There are also directlives which do not have a template. Angular provides some default directive.
 #ngIf - If and else directive. This is a structural directives meaning it changes dom element based on conditions. Angular need to know this. Hence append * when calling this directive.
 <p *ngIf="serverCreated; else servernotcreatedtext">{{ServerCreationStatus}}</p>
<ng-template #servernotcreatedtext>
 <div>
   <p>default text</p>
 </div>


https://www.redhat.com/cms/managed-files/cm-oreilly-kubernetes-patterns-ebook-f19824-201910-en.pdf
https://learning.oreilly.com/library/view/kafka-the-definitive/9781491936153/ch04.html#:~:text=Just%20like%20multiple%20producers%20can,part%20of%20a%20consumer%20group%20.&text=Consumer%20C1%20will%20get%20all%20messages%20from%20all%20four%20T1%20partitions.

Memory analysis
DT="18 19 20 21 22 23 24 25 26 27 28 29"
 >/tmp/sar-$(hostname)-multiple.txt	
 for i in $DT; do
   LC_ALL=C sar -A  -f /var/log/sa/sa$i >> sar-$(hostname)-multiple.txt
 done
 ls -l /tmp/sar-$(hostname)-multiple.txt
 
 
 # Use ksar to see it in graph
 
 
 #IPV4 and IPV6
 
 IPV4 - 2^4 = 32 Bit | Represented in 4 octet(8 bits) | x.x.x.x | total possible combination = 2^32
 
 IPV6 - 2^6 =128 Bit | Represented in 8 hexadecimal(16 bits / base 16) | x.x.x.x.x.x.x.x | total combination = 2 ^128 | shorten ip v6 using  :: meaning it has 1-8 segments with all zeros, the leading zeros in a non zero segment can also be removed for easy reads.
 
 Class less cider block
 10.0.0.0/24 means first 24 bits are frozen and the host address range ( not all  of them can be used) would be 32-24=8 meaning, the first three segment is frozen (10.0.0) and the last segment can be changed from 0-255 giving 256 hostaddress that can be created. So the host address range ( not all  of them can be used) will be from 10.0.0.0 -> 10.0.0.255.
 
 VPC class less cider block
 Maximum number of host address possible in a VPC  = with network mask /16, this will give 2^16=65,536 ip address.
 Minimium number of host address possible in vpc =  with network mask /28, this will give 2^4=16 ip address.
 
 
 GCP PCI Standards
 https://cloud.google.com/architecture/blueprints/google-cloud-pci-gke-review.pdf
 https://d1.awsstatic.com/whitepapers/compliance/pci-dss-compliance-on-aws.pdf?did=wp_card&trk=wp_card
 https://aws.amazon.com/compliance/services-in-scope/  
 https://cloud.google.com/solutions/pci-dss-compliance-in-gcp 
 
