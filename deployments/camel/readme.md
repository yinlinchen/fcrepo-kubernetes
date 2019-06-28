## Camel Toolbox Customizations

The following services have been configured with Fedora credentials:
* Solr indexer
* Triplestore indexer
* Fixity service
* Serialization service
* Reindexing service

The Solr indexer has been configured to communicate with Solr running on port 8080, instead of the default 8983.

The triplestore indexer has been configured to NOT include any `Prefer` headers. The production default is to limit the triples to be indexed by omitting `ldp:contains` triples.

The reindexing service has been configured to bind to the host, `0.0.0.0`, instead of the default `localhost`. This allows the reindexing service to be accessible from outside of the host machine. See [camel-jetty documentation](http://camel.apache.org/jetty.html), search for: "Usage of localhost".

### Deployment
* Deploy Fedora with a file-based objects database:
	* Allocate 10GB storage for Fedora server
	* Mount /mnt/data on the cluster’s Node.
	```
	git clone https://github.com/yinlinchen/fcrepo-kubernetes.git
	cd fcrepo-kubernetes
	kubectl create -f deployments/camel/fcrepo-camel.yaml
	```

### Environment

* [Tomcat 8.x](http://tomcat.apache.org)
    * Available at:  [http://localhost:8080/manager/html](http://localhost:8080/manager/html)
    * Manager username = "fedora4", password = "fedora4"
* [Fedora 5.x](http://fedorarepository.org)
    * Available at: [http://localhost:8080/fcrepo](http://localhost:8080/fcrepo)
    * Authentication/Authorization configuration detailed below
* [Solr 4.10.3](http://lucene.apache.org/solr/)
    * Available at: [http://localhost:8080/solr](http://localhost:8080/solr), for indexing & searching your content.
    * Installed in `/var/lib/tomcat7/solr`
* [Apache Karaf 4.0.5](http://karaf.apache.org/)
    * Installed in `/opt/karaf`
    * Installed as a service `apache-karaf` 
* [Fuseki 2.3.1](http://jena.apache.org/documentation/fuseki2/)
    * Available at: [http://localhost:8080/fuseki](http://localhost:8080/fuseki), for querying and updating.
    * Installed in `/etc/fuseki`
    * Dataset Path name `/test`
    * Persistent storage `/etc/fuseki/databases/test\_data`
* [Fcrepo-camel-toolbox 5.x](https://github.com/fcrepo4-exts/fcrepo-camel-toolbox)
    * Installed in karaf
* [Hawtio 2.5.0](https://hawt.io/)
    * Available at [http://localhost:8181/hawtio](http://localhost:8181/hawtio)
    * Access via username = "karaf", password = "karaf"
    * Installed in karaf

### Hawtio

Hawtio is a pluggable management console for Java stuff which supports any kind of JVM, any kind of container (Tomcat, Jetty, Wildfly, Karaf, etc), and any kind of Java technology and middleware. It has a Camel plugin, that allows you to gain insight into your running Camel applications.

### Fedora Configuration
WebAC authorization is enabled on this Fedora installation.  
The following three Fedora user accounts are available:
 * user account `testuser`, with password `password1`
 * user account `adminuser`, with password `password2`
 * admin account `fedoraAdmin` with the password `secret3`

