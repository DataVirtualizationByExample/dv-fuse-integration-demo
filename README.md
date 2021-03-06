JBoss DV and Fuse (Camel) Integration Demo
======================================
This demo project will get you started with automatically installing two server instances, one with JBoss Data Virtualization and the other with JBoss Fuse, and then configuring 4 examples for accessing data through Camel.
  
  ![Architecture Overview](https://github.com/jbossdemocentral/dv-fuse-integration-demo/blob/master/docs/demo-images/demoarchitectureoverview.png)
  
  Use case 1 - Use the JDBC component to access a virtual database  
  Use case 2 - Use the SQL component to access a virtual database  
  Use case 3 - Use the Olingo component to access a virtual database  
  Use case 4 - Use the REST component to access a virtual database  
  
  NOTE:  Make sure the fabric server passwords for the Maven Plugin is in your ~/.m2/settings.xml file so that the maven plugin can login to the fabric.  See the example in the support/settings.xml file.  Also make sure JAVA_HOME is setup, such as - export JAVA_HOME="/etc/alternatives/java_sdk" - on Fedora.  
  
Local Install Option:  
---------------------    

1. [Download and unzip.](https://github.com/DataVirtualizationByExample/dv-fuse-integration-demo/archive/master.zip).  If running on Windows, it is reccommended the project be extracted to a location near the root drive path due to limitations of length of file/path names.  
  
2. Add the DV and Fuse Products to the software directory.  
  
3. Run 'init.sh' or 'init.bat' to setup the environment locally. 'init.bat' must be run with Administrative privileges.  
  
4. Run 'run.sh' or 'run.bat' to start the servers, create the container and deploy the bundles.  
  
    To run manually instead of the run script you can follow these steps.  
    * Start Fuse - $FUSE_DIR/bin/start  
    * Start DV - $DV_DIR/bin/standalone.sh > dv.log 2>&1 </dev/null &   
    * $FUSE_DIR/bin/client -u admin -p admin "fabric:create --wait-for-provisioning" -r 3  
    * cd projects/usecase1  
    * mvn clean install -DskipTests   
    * mvn fabric8:deploy -DskipTests   
    * cd ../usecase2  
    * mvn clean install -DskipTests  
    * mvn fabric8:deploy -DskipTests   
    * cd ../usecase4  
    * mvn clean install -DskipTests   
    * mvn fabric8:deploy -DskipTests   
    * cp use case 3 config file
    * cd ../..  
    * $FUSE_DIR/bin/client -u admin -p admin "profile-edit --bundles wrap:file://$DV_DIR/dataVirtualization/jdbc/teiid-8.7.1.redhat-8-jdbc.jar usecase1 1.0" -r 3   
    * $FUSE_DIR/bin/client -u admin -p admin "profile-edit --bundles wrap:file://$DV_DIR/dataVirtualization/jdbc/teiid-8.7.1.redhat-8-jdbc.jar usecase2 1.0" -r 3   
    * $FUSE_DIR/bin/client -u admin -p admin "profile-edit --feature camel-olingo2 usecase3" -r 3 
    * $FUSE_DIR/bin/client -u admin -p admin "profile-edit --bundles wrap:file://$DV_DIR/dataVirtualization/jdbc/teiid-8.7.1.redhat-8-jdbc.jar usecase4 1.0" -r 3  
    * $FUSE_DIR/bin/client -u admin -p admin "fabric:container-create-child --profile usecase1 --profile usecase2 --profile usecase4 --profile jboss-fuse-minimal root c1" -r 3   
    * cp $PWD/support/fuse-support/sa.demo.fuse.jdv.usecase3.odata.cfg $FUSE_DIR/instances/c1/etc
  
5. Sign onto the Fuse Management console and check the console log to see the output from the routes for the use cases.  You can also view the Camel Diagrams.  
  
    * You can test the Virtual Database by browsing to http://localhost:8080/odata/CustomerContextVDB/CustomerContextView.CustomerContext?$format=json  
    * Use Case 1 - Since we are using the timer to read from the database and display the results in the log, you can look for the usecase1-context thread which should be route1 logger.  
    * Use Case 2 - Since we are using the timer to read from the database and display the results in the log, you can look for the usecase2-context thread which should be route2 logger.  
    * Use Case 3 - We will update with Fuse 6.2  
    * Use Case 4 - Browse to http://localhost:9000/usecase4 to see the Customer Context data returned from Fuse  
  

Docker Option:  
------------  
  
The demo can be run in a docker container. Full instructions can be found in [support/docker/README.md](support/docker/README.md).  
  
Coming soon:  
------------  
   
   * more demo ideas?  


Supporting Articles  
-------------------  

  [Using a Customer Context with the Camel Components and Data Virtualization](http://www.ossmentor.com/2015/03/using-customer-context-with-fuse.html)

Released versions
-----------------

See the tagged releases for the following versions of the product:

- v1.0.0 - Initial Release and Updates from Bill, Cojan, Kenny; Windows/Docker updates from Andrew
