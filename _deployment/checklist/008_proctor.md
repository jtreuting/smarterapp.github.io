---
title: Proctor Installation Checklist
permalink: "deployment/checklist/proctor.html"
layout: "document"
categories: ["deployment", "checklist", "tds"]
---

# Overview

| Item | Description |
|:-----|:------------|
| Purpose | Provide interfaces for administering assessments |
| Communicates With | OpenAM<br>ProgMan<br>ART<br>TestSpecBank |
| Repository Location | [https://github.com/SmarterApp/TDS_Proctor](https://github.com/SmarterApp/TDS_Proctor){:target="_blank"} |
| Additional Documentation | [README.txt](https://github.com/SmarterApp/TDS_Proctor/blob/master/README.md){:target="_blank"}<br>[proctor-progman-config.txt](https://github.com/SmarterApp/TDS_Proctor/blob/master/proctor/docs/Installation/proctor-progman-config.txt){:target="_blanks"} |

# Instructions

{% include checklist/mysql_setup.md %}

### Create Test Delivery System (TDS) Databases
* Install git:
  * `sudo apt-get install -y git`
* Clone the TDS_Proctor repository from Smarter Balanced GitHub to the server:
  * `git clone https://github.com/SmarterApp/TDS_Proctor.git`
* Unless already done, clone the `TDS_Build` repository from GitHub:
  * `git clone https://github.com/SmarterApp/TDS_Build.git`
* ***NOTE:*** When cloning the repositories above, they should be "siblings" at the same level.  For example, if both
repositories are cloned in the `ubuntu` user's home directory, the directory will look like this:

~~~~
ubuntu@tds-db-deploy:~$ ls -alh
total 48K
drwxr-xr-x  6 ubuntu ubuntu 4.0K May  2 17:57 .
drwxr-xr-x  3 root   root   4.0K May  2 17:22 ..
-rw-r--r--  1 ubuntu ubuntu  220 Apr  9  2014 .bash_logout
-rw-r--r--  1 ubuntu ubuntu 3.6K May  2 17:24 .bashrc
drwx------  2 ubuntu ubuntu 4.0K May  2 17:23 .cache
-rw-------  1 ubuntu ubuntu  135 May  2 17:56 .mysql_history
-rw-r--r--  1 ubuntu ubuntu  675 Apr  9  2014 .profile
drwx------  2 ubuntu ubuntu 4.0K May  2 17:22 .ssh
drwxrwxr-x 13 ubuntu ubuntu 4.0K May  2 17:58 TDS_Build
drwxrwxr-x 12 ubuntu ubuntu 4.0K May  2 17:57 TDS_Proctor
-rw-------  1 ubuntu ubuntu 4.2K May  2 17:54 .viminfo
~~~~

* Navigate to the `TDS_Build\database\tds` directory:
  * `cd TDS_Build\database\tds `
* Update the `db-schema-setup.sh` script to use the correct user name and password (lines 20 and 21):
  * `USER=[a valid MySQL user name that can create databases]`
  * `PW=[the MySQL user's password]`
* If necessary, update the port and hostame (lines 18 and 19)
* Make the `db-schema-setup.sh` executable:
  * `sudo chmod u+x db-schema-setup.sh`
* Run the `db-schema-setup.sh` script to create the TDS database schema and load it with seed data:
  * `./db-schema-setup.sh`

#### Verify the TDS Database Schemas Were Created and Populated With Seed Data
* Log into MySQL:
  * `mysql -u root -p`
* Execute the following query:
  * `show databases;`
* Output should appear as follows:

~~~~
+--------------------+
| Database           |
+--------------------+
| information_schema |
| archive            |
| configs            |
| itembank           |
| mysql              |
| performance_schema |
| session            |
+--------------------+
~~~~

* Change to the `archive` database:
  * `use archive;`
* List the tables in the database:
  * `show tables;`

~~~~
+-----------------------------+
| Tables_in_archive           |
+-----------------------------+
| _dblatency                  |
| _dblatencyarchive           |
| _dblatencyreports           |
| auditaccommodations         |
| opportunityaudit            |
| opportunityclient           |
| serverlatency               |
| serverlatencyarchive        |
| sessionaudit                |
| systemclient                |
| systemerrors                |
| systemerrorsarchive         |
| testopportunityscores_audit |
+-----------------------------+
13 rows in set (0.00 sec)
~~~~

* Change to the `configs` database:
  * `use configs;`
* List the tables in the database:
  * `show tables;`

~~~~
+------------------------------------+
| Tables_in_configs                  |
+------------------------------------+
| __accommodationcache               |
| __appmessagecontexts               |
| __appmessages                      |
| __cachedaccomdepends               |
| __cachedaccommodations             |
| _messageid                         |
| client                             |
| client_accommodationfamily         |
| client_accommodations              |
| client_allowips                    |
| client_externs                     |
| client_fieldtestpriority           |
| client_forbiddenappsexcludeschools |
| client_forbiddenappslist           |
| client_grade                       |
| client_itemscoringconfig           |
| client_language                    |
| client_message                     |
| client_messagearchive              |
| client_messagetranslation          |
| client_messagetranslationaudit     |
| client_pilotschools                |
| client_rtsroles                    |
| client_segmentproperties           |
| client_server                      |
| client_subject                     |
| client_systemflags                 |
| client_tds_rtsattribute            |
| client_tds_rtsattributevalues      |
| client_test_itemconstraint         |
| client_test_itemtypes              |
| client_testeeattribute             |
| client_testeerelationshipattribute |
| client_testeligibility             |
| client_testformproperties          |
| client_testgrades                  |
| client_testkey                     |
| client_testmode                    |
| client_testprerequisite            |
| client_testproperties              |
| client_testrtsspecs                |
| client_testscorefeatures           |
| client_testtool                    |
| client_testtoolrule                |
| client_testtooltype                |
| client_testwindow                  |
| client_timelimits                  |
| client_timewindow                  |
| client_tooldependencies            |
| client_toolusage                   |
| client_voicepack                   |
| geo_clientapplication              |
| geo_database                       |
| rts_role                           |
| statuscodes                        |
| system_applicationsettings         |
| system_browserwhitelist            |
| system_networkdiagnostics          |
| systemerrors                       |
| tds_application                    |
| tds_applicationsettings            |
| tds_browserwhitelist               |
| tds_clientaccommodationtype        |
| tds_clientaccommodationvalue       |
| tds_configtype                     |
| tds_coremessageobject              |
| tds_coremessageuser                |
| tds_fieldtestpriority              |
| tds_role                           |
| tds_systemflags                    |
| tds_testeeattribute                |
| tds_testeerelationshipattribute    |
| tds_testproperties                 |
| tds_testtool                       |
| tds_testtoolrule                   |
| tds_testtooltype                   |
+------------------------------------+
76 rows in set (0.00 sec)
~~~~

* Change to the `itembank` database:
  * `use itembank;`
* List the tables in the database:
  * `show tables;`

~~~~
+---------------------------------------+
| Tables_in_itembank                    |
+---------------------------------------+
| _dblatency                            |
| _synonyms                             |
| _sys_formtestitems                    |
| _testupdate                           |
| aa_itemcl                             |
| affinitygroup                         |
| affinitygroupitem                     |
| alloweditemprops                      |
| configsloaded                         |
| importitemcohorts                     |
| importtestcohorts                     |
| itemmeasurementparameter              |
| itemscoredimension                    |
| loader_errors                         |
| loader_itemscoredimension             |
| loader_itemscoredimensionproperties   |
| loader_measurementparameter           |
| loader_segment                        |
| loader_segmentblueprint               |
| loader_segmentform                    |
| loader_segmentitemselectionproperties |
| loader_segmentpool                    |
| loader_segmentpoolgroupitem           |
| loader_segmentpoolpassageref          |
| loader_setofitemstrands               |
| loader_testblueprint                  |
| loader_testformgroupitems             |
| loader_testformitemgroup              |
| loader_testformpartition              |
| loader_testformproperties             |
| loader_testforms                      |
| loader_testitem                       |
| loader_testitempoolproperties         |
| loader_testitemrefs                   |
| loader_testpackage                    |
| loader_testpackageproperties          |
| loader_testpassages                   |
| loader_testpoolproperties             |
| measurementmodel                      |
| measurementparameter                  |
| performancelevels                     |
| projects                              |
| setoftestgrades                       |
| tbladminstimulus                      |
| tbladminstrand                        |
| tbladminsubjecttestpackage            |
| tblalternatetest                      |
| tblclient                             |
| tblitem                               |
| tblitembank                           |
| tblitemgroup                          |
| tblitemprops                          |
| tblitemselectionparm                  |
| tblsetofadminitems                    |
| tblsetofadminsubjects                 |
| tblsetofitemstimuli                   |
| tblsetofitemstrands                   |
| tblstimulus                           |
| tblstrand                             |
| tblsubject                            |
| tbltestadmin                          |
| tbltestpackage                        |
| testcohort                            |
| testform                              |
| testformitem                          |
+---------------------------------------+
65 rows in set (0.00 sec)
~~~~

* Change to the `session` database:
  * `use session;`
* List the tables in the database:
  * `show tables;`

~~~~
+-----------------------------------+
| Tables_in_session                 |
+-----------------------------------+
| _anonymoustestee                  |
| _externs                          |
| _maxtestopps                      |
| _missingmessages                  |
| _sb_errorlog                      |
| _sb_messagehandler                |
| _sb_messages                      |
| _sb_messagesarchive               |
| _sitelatency                      |
| _synonyms                         |
| adminevent                        |
| admineventitems                   |
| admineventopportunities           |
| alertmessages                     |
| client_os                         |
| client_reportingid                |
| client_sessionid                  |
| clientlatency                     |
| clientlatencyarchive              |
| externs                           |
| ft_groupsamples                   |
| ft_opportunityitem                |
| geo_clientsystem                  |
| geo_session                       |
| itemdistribution                  |
| loadtest_testee                   |
| qareportqueue                     |
| qc_validationexception            |
| r_abnormallogins                  |
| r_blueprintreport                 |
| r_geolatencyreport                |
| r_hourlygeolatencytable           |
| r_hourlyusers                     |
| r_participationcountstable        |
| r_proctorpackage                  |
| r_schoolparticipationreport       |
| r_studentkeyid                    |
| r_studentpackage                  |
| r_studentpackagedetails           |
| r_testcounts                      |
| rtsschoolgrades                   |
| session                           |
| sessiontests                      |
| setofproctoralertmessages         |
| sim_defaultitemselectionparameter |
| sim_itemgroup                     |
| sim_itemselectionparameter        |
| sim_segment                       |
| sim_segmentcontentlevel           |
| sim_segmentitem                   |
| sim_sessiontestpackage            |
| sim_user                          |
| sim_userclient                    |
| simp_itemgroup                    |
| simp_itemselectionparameter       |
| simp_segment                      |
| simp_segmentcontentlevel          |
| simp_segmentitem                  |
| simp_session                      |
| simp_sessiontests                 |
| sirve_audit                       |
| sirve_session                     |
| statuscodes                       |
| tblclsclientsessionstatus         |
| tbluser                           |
| testeeaccommodations              |
| testeeattribute                   |
| testeecomment                     |
| testeehistory                     |
| testeeitemhistory                 |
| testeerelationship                |
| testeeresponse                    |
| testeeresponsearchive             |
| testeeresponseaudit               |
| testeeresponsescore               |
| testoppabilityestimate            |
| testopportunity                   |
| testopportunity_readonly          |
| testopportunitycontentcounts      |
| testopportunityscores             |
| testopportunitysegment            |
| testopportunitysegmentcounts      |
| testopprequest                    |
| testopptoolsused                  |
| timelimits                        |
+-----------------------------------+
85 rows in set (0.00 sec)
~~~~

* Change to the `itembank` database:
  * `use itembank;`
* List the assessments that have been loaded:
  * `select _key, testid from tblsetofadminsubjects;`

~~~~
+------------------------------------------------+------------------------------+
| _key                                           | testid                       |
+------------------------------------------------+------------------------------+
| (SBAC_PT)SBAC ELA 3-ELA-3-Spring-2014-2015     | SBAC ELA 3-ELA-3             |
| (SBAC_PT)SBAC ELA 6-ELA-6-Spring-2014-2015     | SBAC ELA 6-ELA-6             |
| (SBAC_PT)SBAC ELA HS-ELA-10-Spring-2014-2015   | SBAC ELA HS-ELA-10           |
| (SBAC_PT)SBAC Math 3-MATH-3-Spring-2014-2015   | SBAC Math 3-MATH-3           |
| (SBAC_PT)SBAC Math 6-MATH-6-Spring-2014-2015   | SBAC Math 6-MATH-6           |
| (SBAC_PT)SBAC Math 7-MATH-7-Spring-2014-2015   | SBAC Math 7-MATH-7           |
| (SBAC_PT)SBAC Math HS-MATH-10-Spring-2014-2015 | SBAC Math HS-MATH-10         |
| (SBAC_PT)SBAC-Student Help-11-Spring-2014-2015 | SBACTraining-Student Help-11 |
+------------------------------------------------+------------------------------+
8 rows in set (0.00 sec)
~~~~

***NOTE:*** The tests loaded into the `itembank` database come from the [Practice and Training Tests Package](ftp://ftps.smarterbalanced.org/~sbacpublic/Public/PracticeAndTrainingTests/2015-08-28_TrainingTestPackagesAndContent.zip){:target="_blank"}.

## Create AWS Web Application Instance
* Create server instance to host the Permissions component
  * Select an image with the **Ubuntu 14.04 LTS 64-bit**{: style="color: #04384e"} operating system
* Create or choose an AWS security group with the following ports for inbound TCP traffic (can be done during instance creation):
  * 22
  * 80
  * 443
  * 1043
  * 8080
  * 8084
  * 8443

## Proctor Application Setup
* Update package manager:
  * `sudo apt-get update`
  * `sudo apt-get upgrade -y`

* Install packages to satisfy dependencies:
  * `sudo apt-get install -y ntp mercurial openjdk-7-jdk`

{% include checklist/tomcat_setup.md %}

{% include checklist/generate_keystore.md %}

### Configure Proctor in ProgMan
{% include checklist/progman_config.md %}

* Shown below are the Proctor properties that need to be configured in ProgMan:

* `component.name=`Proctor
* `datasource.maxPoolSize=`20
* `datasource.minPoolSize=`5
* `datasource.password=`[*Password for the MySQL user account*{: style="color: red"}]
* `datasource.url=`jdbc:mysql://[*FQDN or IP address of MySQL database server that hosts the TDS databases*{: style="color: red"}]/session
* `datasource.username=`[*MySQL user account that has access to read/write in the TDS databases*{: style="color: red"}]
* `EncryptionKey=`[*A value to use as an encryption key.  Must be at least 24 characters long.  Example value: Thisisanincrediblylongkeythatiscertainlylongerthantwentyfourcharacters*{: style="color: red"}]
* `iris.ContentPath=`[*Path to where assessment content will be stored.  If following this guide, set to **/usr/local/tomcat/content/***{: style="color: red"}]
* `itemScoring.callbackUrl=`http://localhost:8080/student/ItemScoringCallback.axd
* `itemscoring.qti.sympyMaxTries=`3
* `itemscoring.qti.sympyServiceUrl=`[*URL to where the Python equation scoring application is hostsed.  If following this guide, set to **http://localhost:8084/***{: style="color: red"}]
* `itemscoring.qti.sympyTimeoutMillis=`[*Number of milliseconds before the Python equation scoring application times out*{: style="color: red"}]
* `logLatencyInterval=`55
* `logLatencyMaxTime=`30000
* `mna.logger.level=`ERROR
* `mna.mnaUrl=`http://<mna-context-url>/mna-rest/
* `mna.oauth.client.id=`[*The OAuth client name for the Monitoring and Alerting (MnA) component; can use a "common" OAuth client name, e.g. one OAuth client for multiple components*{: style="color: red"}]
* `mna.oauth.client.secret=`[*Password for OAuth client used for MnA.  Starting value is sbac12345*{: style="color: red"}]
* `mnaNodeName=`dev
* `mnaServerName=`proctor_dev
* `oauth.access.url=`https://[*FQDN or IP address of the OpenAM server*{: style="color: red"}]/auth/oauth2/access_token?realm=/sbac
* `oauth.testreg.client.granttype=`password
* `oauth.testreg.client.secret=`[*Password for OAuth client used for ART.  Starting value is sbac12345*{: style="color: red"}]
* `oauth.testreg.client=`[*The OAuth client name for the ART component; can use a "common" OAuth client name, e.g. one OAuth client for multiple components*{: style="color: red"}]
* `oauth.testreg.client.id=`[*The OAuth client name for the ART component; can use a "common" OAuth client name, e.g. one OAuth client for multiple components*{: style="color: red"}]
* `oauth.testreg.password=`[*Password for user account that exists in OpenDJ*{: style="color: red"}]
* `oauth.testreg.username=`[*Email address of user account that exists in OpenDJ*{: style="color: red"}]
* `oauth.tsb.client.secret=`[*Password for OAuth client used for TestSpecBank.  Starting value is sbac12345*{: style="color: red"}]
* `oauth.tsb.client=`[*The OAuth client name for the TestSpecBank component; can use a "common" OAuth client name, e.g. one OAuth client for multiple components*{: style="color: red"}]
* `permission.security.profile=`[*Environment name for Proctor component in ProgMan*{: style="color: red"}]
* `permission.uri=`http://[*FQDN or IP address for the Permissions component*{: style="color: red"}]/rest
* `proctor.ClientName=`[*Name of client that "owns" assessments in database.  Set to **SBAC_PT** for training tests, set to **SBAC** for all others*{: style="color: red"}]
* `proctor.oauth.checktoken.endpoint=`https://[*FQDN or IP address of the OpenAM server*{: style="color: red"}]/auth/oauth2/tokeninfo?realm=/sbac
* `proctor.oauth.resource.client.id=`[*The OAuth client name for the Proctor component; can use a "common" OAuth client name, e.g. one OAuth client for multiple components*{: style="color: red"}]
* `proctor.oauth.resource.client.secret=`[*Password for OAuth client used for Proctor.  Starting value is sbac12345*{: style="color: red"}]
* `proctor.security.dir=`file:////opt/sbtds/proctor/resources/security
* `proctor.security.idp=`https://[*FQDN or IP address of the OpenAM server*{: style="color: red"}]/auth/saml2/jsp/exportmetadata.jsp?realm=/sbac
* `proctor.security.saml.alias=`[*The Service Provider name for the Proctor component*{: style="color: red"}]
* `proctor.security.saml.keystore.cert=`[*The name of the private key that was added to the samlKeystore.jks*{: style="color: red"}]
* `proctor.security.saml.keystore.pass=`[*The password for accessing the samlKeystore.jks*{: style="color: red"}]
* `proctor.StateCode=`[*The name of the STATE-level Client in ProgMan*{: style="color: red"}]
* `proctor.TestRegistrationApplicationUrl=`http://[*FQDN or IP address of Permissions component*{: style="color: red"}]/rest
* `proctor.webapp.saml.metadata.filename=`[*Name of file that stores SAML data for ART Web Application.*{: style="color: red"}]
* `proctor.SessionType=`[*The type of Session; should always be set to **0***{: style="#f00;"}]
* `proctor.TDSArchiveDBName=`[*The name of the TDS Archive database on the MySQL server; should be set to **archive***{: style="color: #f00;"}]
* `proctor.TDSConfigsDBName=`[*The name of the TDS Configuration database on the MySQL server; should be set to **configs***{: style="color: #f00;"}]
* `proctor.TDSSessionDBName=`[*The name of the TDS Session database on the MySQL server; should be set to **session***{: style="color: #f00;"}]

* Example ProgMan properties for Proctor:

<div class="highlighter-rouge">
<pre class="highlight">
<code>component.name=Proctor
datasource.maxPoolSize=20
datasource.minPoolSize=5
datasource.password=<span class="placeholder-example">[redacted]</span>
datasource.url=jdbc:mysql://<span class="placeholder-example">52.33.126.67</span>/session
datasource.username=<span class="placeholder-example">remoteuser</span>
EncryptionKey=Thisisanincrediblylongkeythatiscertainlylongerthantwentyfourcharacters
iris.ContentPath=<span class="placeholder-example">/usr/local/tomcat/content/</span>
itemScoring.callbackUrl=http://localhost:8080/student/ItemScoringCallback.axd
itemscoring.qti.sympyMaxTries=3
itemscoring.qti.sympyServiceUrl=http://localhost:8084/
itemscoring.qti.sympyTimeoutMillis=10000
logLatencyInterval=55
logLatencyMaxTime=30000
mna.logger.level=ERROR
mna.mnaUrl=http://&lt;mna-context-url&gt;/mna-rest/
mna.oauth.client.id=mna
mna.oauth.client.secret=<span class="placeholder-example">[redacted]</span>
mnaNodeName=dev
mnaServerName=proctor_dev
oauth.access.url=https://<span class="placeholder-example">sso-dev.sbtds.org</span>/auth/oauth2/access_token?realm=/sbac
oauth.testreg.client.granttype=password
oauth.testreg.client.secret=<span class="placeholder-example">[redacted]</span>
oauth.testreg.client=<span class="placeholder-example">pm</span>
oauth.testreg.client.id=<span class="placeholder-example">pm</span>
oauth.testreg.password=<span class="placeholder-example">[redacted]</span>
oauth.testreg.username=<span class="placeholder-example">primeuser@fairwaytech.com</span>
oauth.tsb.client.secret=<span class="placeholder-example">[redacted]</span>
oauth.tsb.client=<span class="placeholder-example">tsb</span>
permission.security.profile=<span class="placeholder-example">dev</span>
permission.uri=http://<span class="placeholder-example">52.32.19.35:8080</span>/rest
proctor.ClientName=SBAC
proctor.oauth.checktoken.endpoint=https://<span class="placeholder-example">sso-dev.sbtds.org</span>/auth/oauth2/tokeninfo?realm=/sbac
proctor.oauth.resource.client.id=<span class="placeholder-example">pm</span>
proctor.oauth.resource.client.secret=<span class="placeholder-example">[redacted]</span>
proctor.security.dir=file:////var/lib/tomcat7/resources/security
proctor.security.idp=https://<span class="placeholder-example">sso-dev.sbtds.org</span>/auth/saml2/jsp/exportmetadata.jsp?realm=/sbac
proctor.security.saml.alias=<span class="placeholder-example">proctorlocal</span>
proctor.security.saml.keystore.cert=<span class="placeholder-example">proctor-saml-sp</span>
proctor.security.saml.keystore.pass=<span class="placeholder-example">[redacted]</span>
proctor.StateCode=<span class="placeholder-example">OR</span>
proctor.TestRegistrationApplicationUrl=http://<span class="placeholder-example">52.32.59.74:8080</span>/rest
proctor.webapp.saml.metadata.filename=<span class="placeholder-example">proctor_sp.xml</span>
proctor.sessionType=0
proctor.TDSArchiveDBName=<span class="placeholder-example">archive</span>
proctor.TDSConfigsDBName=<span class="placeholder-example">configs</span>
proctor.TDSSessionDBName=<span class="placeholder-example">session</span></code>
</pre>
</div>

### Deploy Proctor Components

#### Configure Tomcat
* Stop the Tomcat server:
  * `sudo service tomcat7 stop`

{% include checklist/tomcat_java_opts.md %}

* Example `JAVA_OPTS` for TDS application server:

<div class="highlighter-rouge">
<pre class="highlight">
<code>JAVA_OPTS="-Djava.awt.headless=true\
 -XX:+UseConcMarkSweepGC\
 -Xms<span class="placeholder-example">1024m</span>\
 -Xmx<span class="placeholder-example">4096m</span>\
 -XX:PermSize=<span class="placeholder-example">512m</span>\
 -XX:MaxPermSize=<span class="placeholder-example">1512m</span>\
 -DSB11_CONFIG_DIR=$CATALINA_BASE/resources\
 -Dprogman.baseUri=<span class="placeholder-example">http://52.34.140.123:8080</span>/rest/\
 -Dspring.profiles.active=mna.client.null,progman.client.impl.integration,server.singleinstance\
 -Dprogman.locator=<span class="placeholder-example">tds</span>,<span class="placeholder-example">Development</span>"</code>
</pre>
</div>

#### Create Proctor Log File Directories
* Create a directory for Proctor log file:
  * `sudo mkdir -p /usr/share/tomcat7/logs/proctor`
  * `sudo chown -R tomcat7:tomcat7 /usr/share/tomcat7/logs`
* ***OPTIONAL:***  Create links in the Tomcat log directory to the Web Application log file:
  * `sudo ln -s /usr/share/tomcat7/logs/proctor/proctor.log /var/lib/tomcat7/logs/proctor.log`

{% include checklist/tomcat_install_mysql_connector.md %}

#### Download War Files
* Download the latest `.war` file for the Proctor Component into the Tomcat server's `webapps` directory:
  * `sudo wget https://github.com/SmarterApp/TDS_Proctor/releases/download/1.1.0/testadmin-1.1.0.war -O /var/lib/tomcat7/webapps/ROOT.war`
  * Example:
    * `sudo wget https://github.org/SmarterApp/TDS_Proctor/releases/download/1.1.0/`<span class="placeholder-example">testadmin-1.1.0.war</span>` -O /var/lib/tomcat7/webapps/ROOT.war`

{% include checklist/tomcat_pm_client_security_props.md %}

* Start Tomcat to expand the deployed `.war` files:
  * `sudo service tomcat7 start`

{% include checklist/saml_setup.md %}

{% include checklist/saml_registration.md %}

[back to Deployment Checklists](index.html)