---
layout: post
comments: true
permalink: /:title
title: 'Container Services'
author: Richard
date: 2007/04/30
alias: /container-services
tags:
---

You don't need to use all of [Java EE][] to build an enterprise
application. Partly because of that I've always been nervous of big EE
application servers such as JBoss, Glassfish, OC4J, WebLogic, WebSphere,
and instead I've preferred mixing and matching open source libraries and
stand-alone components. Or maybe I just don't need [the features][], or
I've not run into scalability or transaction control that containers
help manage, or maybe I'm not convinced on the benefits of a "single
solution". So although I have ended up using them for some projects, I'm
not sold on the need and I've never really thought they make life
easier.

But lately I've started to come round to handing some responsibilities
off to containers. One responsibility is web user authentication (a.k.a.
login). It's a great example of saving time and improving quality, by
leaving those aspects to tried-and-tested implementations provided by
the application server. Unfortunately, [form based authentication][] in
the EE spec isn't flexible enough. Most people I know end up rolling
their own solutions, which is a shame. Maybe it'll be usable one day...

There is one good example I've found where you can leave something to
the container. If you have an application to deploy, and you want to
deploy it on a test machine and a production machine, it's very
convenient to leave the database configuration to the container. That
is: you build you application (say, a WAR file) and then you can drop it
into a container on a QA machine and it'll pick up the connection
settings for the QA database; drop it into the live environment and
it'll pick up the live database settings. The configuration is in the
container (once), not the application (each build), and so [Mr
Cock-Up][] is less likely to visit.

Here's how you do it with Tomcat 5.5 and Hibernate 3.0.x:

1.  Copy your JDBC driver and your connection pool into
    `$TOMCAT/common/lib/` e.g.,
    `mysql-connector-java-3.0.14-production-bin.jar` and
    `c3p0-0.9.0.4.jar` for example.
2.  In Tomcat's `server.xml`, in the `<GlobalNamingResources>` section
    add: 
    
	    <Resource name="jdbc/MYPROJECT-database" 
	     description="My Project Database Connection" driverClass="com.mysql.jdbc.Driver"
	     maxPoolSize="25"
	     minPoolSize="2"
	     acquireIncrement="1"
	     auth="Container"
	     maxStatements="50"
	     idleConnectionTestPeriod="3600"
	     testConnectionOnCheckin="true"
	     automaticTestTable="connection_test"
	     maxIdleTime="21600"
	     factory="org.apache.naming.factory.BeanFactory"
	     type="com.mchange.v2.c3p0.ComboPooledDataSource"
	     jdbcUrl="jdbc:mysql://localhost:3306/MYPROJECT?useUnicode=true&
	     amp;characterEncoding=UTF-8&autoReconnect=true"
	     user="myproj" password="trustno1" />
    
    Adjust parameters to taste. What's going on here, is that the Apache
    `BeanFactory` is creating an instance of `ComboPooledDataSource` and
    then trying to call `setX` methods 
    on the instance from the attributes in this XML snippet. So check
    the API for your connection pool implementation to see what you can
    set.
3.  Create the `webapp/META-INF/context.xml` (or similar) with this
    content: 

    
	    <Context>
	     <ResourceLink name="jdbc/MYPROJECT-database" 
	     global="jdbc/MYPROJECT-database" 
	     type="javax.sql.DataSource" />
	    </Context>

    That tells Tomcat that this web application wants to access a global
    Tomcat resource called jdbc/MYPROJECT-database. 
    When Tomcat deploys the WAR this file is copied to
    `TOMCAT/conf/Catalina/localhost/YOUR-WAR-NAME.xml`.

4.  If you're using Hibernate, modify the config file to ask the
    container for the connection: 
    
	     <property name="dialect">
	       org.hibernate.dialect.MySQLDialect
	     </property>
	     <property name="hibernate.connection.datasource">
	       java:comp/env/jdbc/MYPROJECT-database
	     </property>
    
    ...ensuring that all the other database connection stuff (username
    etc) are removed.

That's it. Then when you run, if Hibernate needs a connection it asks
Tomcat for jdbc/MYPROJECT-database and Tomcat gives back whatever the
instance is set up to provide. Looks like it's doable in [Jetty][] too.



  [Java EE]: http://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition
  [the features]: http://java.sun.com/javaee/overview/faq/javaee_faq.jsp#benefits
  [form based authentication]: http://www.onjava.com/lpt/a/1024
  [Mr Cock-Up]: http://www.bbc.co.uk/comedy/blackadder/epguide/two_head.shtml
  [Jetty]: http://docs.codehaus.org/display/JETTY/JNDI
