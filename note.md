
Maven
-----
Source: http://maven.apache.org/general.html#plugin-version

#### How do I determine what version of a plugin I am using?
>You can use the Maven Help Plugin's describe goal. For example, to find out the version of the install plugin:
`
mvn help:describe -DgroupId=org.codehaus.mojo -DartifactId=aspectj-maven-plugin  
mvn -Dplugin=install help:describe  
`
>Note that you must give the plugin prefix as the argument to plugin, not it's artifact ID.


> Configuration de camel 
https://issues.apache.org/jira/browse/CAMEL-6562?jql=project%20%3D%20CAMEL%20AND%20text%20~%20%22restlet%20connection%22



> Written with [StackEdit](http://benweet.github.io/stackedit/).
