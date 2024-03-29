# Config.ini file
  
  * Configured all up to Level 3 TLS, parameters to note inside config.ini file.
    * use_tls
	* verify_cert_file
	* client_key_file
	* client_keypw_file
	* client_cert_file
  * Besides config.ini file, explanation of setting up TLS.

    ```bash
     [General]
     # Hostname of the CM server.
     server_host=ip-172-31-33-245.eu-central-1.compute.internal
     
     # Port that the CM server is listening on.
     server_port=7182
     
     ## It should not normally be necessary to modify these.
     # Port that the CM agent should listen on.
     # listening_port=9000
     
     # IP Address that the CM agent should listen on.
     # listening_ip=
     
     # Hostname that the CM agent reports as its hostname. If unset, will be
     # obtained in code through something like this:
     #
     #   python -c 'import socket; \
     #              print socket.getfqdn(), \
     #                    socket.gethostbyname(socket.getfqdn())'
     #
     # listening_hostname=
     
     # An alternate hostname to report as the hostname for this host in CM.
     # Useful when this agent is behind a load balancer or proxy and all
     # inbound communication must connect through that proxy.
     # reported_hostname=
     
     # Port that supervisord should listen on.
     # NB: This only takes effect if supervisord is restarted.
     # supervisord_port=19001
     
     # Log file.  The supervisord log file will be placed into
     # the same directory.  Note that if the agent is being started via the
     # init.d script, /var/log/cloudera-scm-agent/cloudera-scm-agent.out will
     # also have a small amount of output (from before logging is initialized).
     # log_file=/var/log/cloudera-scm-agent/cloudera-scm-agent.log
     
     # Persistent state directory.  Directory to store CM agent state that
     # persists across instances of the agent process and system reboots.
     # Particularly, the agent's UUID is stored here.
     # lib_dir=/var/lib/cloudera-scm-agent
     
     # Parcel directory.  Unpacked parcels will be stored in this directory.
     # Downloaded parcels will be stored in <parcel_dir>/../parcel-cache
     # parcel_dir=/opt/cloudera/parcels
     
     # Enable supervisord event monitoring.  Used in eager heartbeating, amongst
     # other things.
     # enable_supervisord_events=true
     
     # Maximum time to wait (in seconds) for all metric collectors to finish
     # collecting data.
     max_collection_wait_seconds=10.0
     
     # Maximum time to wait (in seconds) when connecting to a local role's
     # webserver to fetch metrics.
     metrics_url_timeout_seconds=30.0
     
     # Maximum time to wait (in seconds) when connecting to a local TaskTracker
     # to fetch task attempt data.
     task_metrics_timeout_seconds=5.0
     
     # The list of non-device (nodev) filesystem types which will be monitored.
     monitored_nodev_filesystem_types=nfs,nfs4,tmpfs
     
     # The list of filesystem types which are considered local for monitoring purposes.
     # These filesystems are combined with the other local filesystem types found in
     # /proc/filesystems
     local_filesystem_whitelist=ext2,ext3,ext4
     
     # The largest size impala profile log bundle that this agent will serve to the
     # CM server. If the CM server requests more than this amount, the bundle will
     # be limited to this size. All instances of this limit being hit are logged to
     # the agent log.
     impala_profile_bundle_max_bytes=1073741824
     
     # The largest size stacks log bundle that this agent will serve to the CM
     # server. If the CM server requests more than this amount, the bundle will be
     # limited to this size. All instances of this limit being hit are logged to the
     # agent log.
     stacks_log_bundle_max_bytes=1073741824
     
     # The size to which the uncompressed portion of a stacks log can grow before it
     # is rotated. The log will then be compressed during rotation.
     stacks_log_max_uncompressed_file_size_bytes=5242880
     
     # The orphan process directory staleness threshold. If a diretory is more stale
     # than this amount of seconds, CM agent will remove it.
     orphan_process_dir_staleness_threshold=5184000
     
     # The orphan process directory refresh interval. The CM agent will check the
     # staleness of the orphan processes config directory every this amount of
     # seconds.
     orphan_process_dir_refresh_interval=3600
     
     # A knob to control the agent logging level. The options are listed as follows:
     # 1) DEBUG (set the agent logging level to 'logging.DEBUG')
     # 2) INFO (set the agent logging level to 'logging.INFO')
     scm_debug=INFO
     
     # The DNS resolution collecion interval in seconds. A java base test program
     # will be executed with at most this frequency to collect java DNS resolution
     # metrics. The test program is only executed if the associated health test,
     # Host DNS Resolution, is enabled.
     dns_resolution_collection_interval_seconds=60
     
     # The maximum time to wait (in seconds) for the java test program to collect
     # java DNS resolution metrics.
     dns_resolution_collection_timeout_seconds=30
     
     # The directory location in which the agent-wide kerberos credential cache
     # will be created.
     # agent_wide_credential_cache_location=/var/run/cloudera-scm-agent
     
     [Security]
     # Use TLS and certificate validation when connecting to the CM server.
     use_tls=1
     
       
     # The maximum allowed depth of the certificate chain returned by the peer.
     # The default value of 9 matches the default specified in openssl's
     # SSL_CTX_set_verify.
     max_cert_depth=9
     
     # A file of CA certificates in PEM format. The file can contain several CA
     # certificates identified by
     #
     # -----BEGIN CERTIFICATE-----
     # ... (CA certificate in base64 encoding) ...
     # -----END CERTIFICATE-----
     #
     # sequences. Before, between, and after the certificates text is allowed which
     # can be used e.g. for descriptions of the certificates.
     #
     # The file is loaded once, the first time an HTTPS connection is attempted. A
     # restart of the agent is required to pick up changes to the file.
     #
     # Note that if neither verify_cert_file or verify_cert_dir is set, certificate
     # verification will not be performed.
     
     verify_cert_file=/opt/cloudera/security/x509/cmhost-cert.pem
     
     # Directory containing CA certificates in PEM format. The files each contain one
     # CA certificate. The files are looked up by the CA subject name hash value,
     # which must hence be available. If more than one CA certificate with the same
     # name hash value exist, the extension must be different (e.g. 9d66eef0.0,
     # 9d66eef0.1 etc). The search is performed in the ordering of the extension
     # number, regardless of other properties of the certificates. Use the c_rehash
     # utility to create the necessary links.
     #
     # The certificates in the directory are only looked up when required, e.g. when
     # building the certificate chain or when actually performing the verification
     # of a peer certificate. The contents of the directory can thus be changed
     # without an agent restart.
     #
     # When looking up CA certificates, the verify_cert_file is first searched, then
     # those in the directory. Certificate matching is done based on the subject name,
     # the key identifier (if present), and the serial number as taken from the
     # certificate to be verified. If these data do not match, the next certificate
     # will be tried. If a first certificate matching the parameters is found, the
     # verification process will be performed; no other certificates for the same
     # parameters will be searched in case of failure.
     #
     # Note that if neither verify_cert_file or verify_cert_dir is set, certificate
     # verification will not be performed.
     # verify_cert_dir=
     
     # PEM file containing client private key.
     # client_key_file=
     
     # A command to run which returns the client private key password on stdout
     # client_keypw_cmd=
     
     # If client_keypw_cmd isn't specified, instead a text file containing
     # the client private key password can be used.
     # client_keypw_file=
     
     # PEM file containing client certificate.
     # client_cert_file=
     
     ## Location of Hadoop files.  These are the CDH locations when installed by
     ## packages.  Unused when CDH is installed by parcels.
     [Hadoop]
     #cdh_crunch_home=/usr/lib/crunch
     #cdh_flume_home=/usr/lib/flume-ng
     #cdh_hadoop_bin=/usr/bin/hadoop
     #cdh_hadoop_home=/usr/lib/hadoop
     #cdh_hbase_home=/usr/lib/hbase
     #cdh_hbase_indexer_home=/usr/lib/hbase-solr
     #cdh_hcat_home=/usr/lib/hive-hcatalog
     #cdh_hdfs_home=/usr/lib/hadoop-hdfs
     #cdh_hive_home=/usr/lib/hive
     #cdh_httpfs_home=/usr/lib/hadoop-httpfs
     #cdh_hue_home=/usr/share/hue
     #cdh_hue_plugins_home=/usr/lib/hadoop
     #cdh_impala_home=/usr/lib/impala
     #cdh_llama_home=/usr/lib/llama
     #cdh_mr1_home=/usr/lib/hadoop-0.20-mapreduce
     #cdh_mr2_home=/usr/lib/hadoop-mapreduce
     #cdh_oozie_home=/usr/lib/oozie
     #cdh_parquet_home=/usr/lib/parquet
     #cdh_pig_home=/usr/lib/pig
     #cdh_solr_home=/usr/lib/solr
     #cdh_spark_home=/usr/lib/spark
     #cdh_sqoop_home=/usr/lib/sqoop
     #cdh_sqoop2_home=/usr/lib/sqoop2
     #cdh_yarn_home=/usr/lib/hadoop-yarn
     #cdh_zookeeper_home=/usr/lib/zookeeper
     #hive_default_xml=/etc/hive/conf.dist/hive-default.xml
     #webhcat_default_xml=/etc/hive-webhcat/conf.dist/webhcat-default.xml
     #jsvc_home=/usr/libexec/bigtop-utils
     #tomcat_home=/usr/lib/bigtop-tomcat
     
     ## Location of Cloudera Management Services files.
     [Cloudera]
     #mgmt_home=/usr/share/cmf
     
     ## Location of JDBC Drivers.
     [JDBC]
     #cloudera_mysql_connector_jar=/usr/share/java/mysql-connector-java.jar
     #cloudera_oracle_connector_jar=/usr/share/java/oracle-connector-java.jar
     #By default, postgres jar is found dynamically in $MGMT_HOME/lib
     #cloudera_postgresql_jdbc_jar=
    ```

# See issue [#6](/../../issues/6)
During lab i've faced some typos, and missconfigurations. On top of which caching of my browser produced additional trouble when i tried to reverse to configuration pre TLS Level 1 exercise.
That led me to some desperate measures, like manually adding inserts inside scm.CONFIGS table, in order to get back into state where Cloudera Manager was working in HTTPS mode.
Misconfigurations led to problem, of Cloudera Agents not being able to talk to CM Server.

In the end, after fixing problems, did all configs for TLS level 3.

## Procedure taken:
* Steps taken on CM Server Node
```bash
 export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
 export PATH=$JAVA_HOME/bin:PATH
 
 mkdir -p /opt/cloudera/security/x509/ /opt/cloudera/security/jks/ ~/certificates
 
 chown -R cloudera-scm:cloudera-scm /opt/cloudera/security/jks
 
 keytool -storepasswd -keystore /usr/java/jdk1.7.0_67-cloudera/jre/lib/security/jssecacerts
 (from changeit to cloudera) 
 
 umask 0700
 
 cd /opt/cloudera/security/jks
 
 keytool -genkeypair -alias $(hostname -f)-server -keyalg RSA -keysize 2048 -dname "CN=$(hostname -f),OU=Engineering,O=Cloudera,L=Palo Alto,ST=California,C=US" -keypass cloudera -keystore $(hostname -f)-server.jks -storepass cloudera
 
 cp $JAVA_HOME/jre/lib/security/cacerts $JAVA_HOME/jre/lib/security/jssecacerts 
 
 keytool -export -alias $(hostname -f)-server -keystore $(hostname -f)-server.jks -rfc -file $(hostname -f)-server.cer
 
 cp *.cer /opt/cloudera/security/x509/$(hostname -f)-server.pem
 
 cp *.cer /opt/cloudera/security/x509/cmhost-cert.pem
 
 mv *.cer ~/certificates
 
 keytool -import -alias $(hostname -f)-server -file /opt/cloudera/security/jks/*.cer -keystore $JAVA_HOME/jre/lib/security/jssecacerts -storepass cloudera 
 (answer Yes)
```

* Steps taken on other nodes
```bash
 export JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
 export PATH=$JAVA_HOME/bin:PATH
 
 mkdir -p /opt/cloudera/security/x509/ /opt/cloudera/security/jks/
 
 chown -R cloudera-scm:cloudera-scm /opt/cloudera/security/jks
 
 umask 0700
 
 cd /opt/cloudera/security/jks
 
 keytool -genkeypair -alias $(hostname -f)-server -keyalg RSA -keysize 2048 -dname "CN=$(hostname -f),OU=Engineering,O=Cloudera,L=Palo Alto,ST=California,C=US" -keypass cloudera -keystore $(hostname -f)-server.jks -storepass cloudera
 
 keytool -export -alias $(hostname -f)-server -keystore $(hostname -f)-server.jks -rfc -file $(hostname -f)-server.cer
 
 cp *.cer /opt/cloudera/security/x509/$(hostname -f)-server.pem
 
 scp ip-172-31-33-245.eu-central-1.compute.internal:/opt/cloudera/security/x509/cmhost-cert.pem /opt/cloudera/security/x509/cmhost-cert.pem
 (needed for Agents to authenticate with CM keystore)
 
 scp *.cer ip-172-31-33-245.eu-central-1.compute.internal:~/all_certificates
 (from all nodes sent certificates towards CM machine into dir ~/all_certificates, so that can be loaded into another keystore for TLS level 3)
```
* Loadeding .crt'
  *every cert from ~/all_certificates into /usr/java/jdk1.7.0_67-cloudera/jre/lib/security/jssecacerts, besides ip-172-31-33-245.eu-central-1.compute.internal-server.crt, since previously it was loaded.
```
 cd ~/all_certificates
 (then for every .cer file)
 keytool -import -alias {filename without .cer extension} -file {full filename with extension} -keystore $JAVA_HOME/jre/lib/security/jssecacerts -storepass cloudera
```

* For Agent Authentication, replace parameters:
  * client_key_file
  ```
   sed -i "s/#\ client_key_file=/client_key_file=\/opt\/cloudera\/security\/jks\/$(hostname -f)-server.jks/g" /etc/cloudera-scm-agent/config.ini | grep client_key
  ```
  * make and link inside config.ini client_keypw_file
  ```
   echo "cloudera" > /etc/cloudera-scm-agent/agentkey.pw
   sed -i "s/#\ client_keypw_file=/client_keypw_file=\/etc\/cloudera-scm-agent\/agentkey.pw/g" /etc/cloudera-scm-agent/config.ini
   sed -i "s/#\ client_cert_file=/client_cert_file=\/opt\/cloudera\/security\/x509\/$(hostname -f)-server.pem/g" /etc/cloudera-scm-agent/config.ini
  ```

![Image of CMSettings](../screnshots/TLS.PNG)

* Restart Cloudera Manager Server
```bash
 systemctl restart cloudera-scm-server
```

* Restart Cloudera Manager Agent on every node
```bash
 systemctl restart cloudera-scm-agent
```
 
 # IMPORTANT NOTE
 
 * TLS Level 2 works, TLS Level 3 didn't work. 
 ```
  What i am assuming is that , all keys msy be signed by certificate authority, and loaded into JKS, while mine are loaded into mutual truststore, but are not signed by same authority.
 ```
 * Working TLS Level 2
 ![Image of TLS_Level_2](../screnshots/tls_level_2.PNG)