---
layout: post
comments: true
permalink: /:title.html
title: 'Shell aliases for Lift development'
author: Richard
date: 2010/03/25
alias: /shell-aliases-for-lift-development
tags:
---

Last night I was asked about the aliases I used during my Lift talk.
 Here they are...

    $ cat ~/.profile 
    export REBEL_JAR=/Users/richard/Developer/libs/javarebel/jrebel-2.1a/jrebel.jar  
    export MAVEN_HOME=/Users/richard/Applications/maven/apache-maven-2.2.1 
    export MAVEN_OPTS="-Xmx256M -XX:MaxPermSize=256m -Dfile.encoding=utf8" 
    export JAVA_OPTS="-Dfile.encoding=utf8" 
    export PATH=$MAVEN_HOME/bin:$PATH  
    
    # E.g., "m package" to run the Maven package target in offline mode: 
    alias m='mvn -o'   
    
    # Run Maven in a mode that fetches sources and documentation for libraries: 
    alias mm='mvn -DdownloadJavadocs=true -DdownloadSources=true'   
    
    # Start your Lift application in the REPL 
    alias liftconsole='mvn -o scala:console -DmainConsole=LiftConsole'   
    
    # Start your Lift application using JRebel 
    alias mmm='MAVEN_OPTS="-Dfile.encoding=utf8 -XX:MaxPermSize=256m -Djetty.scanIntervalSeconds=0 -noverify -javaagent:$REBEL_JAR" mvn -o jetty:run'   
    
    # Create a new 2.0-M2 Lift application 
    alias liftcreate="mvn archetype:generate -DarchetypeGroupId=net.liftweb -DarchetypeArtifactId=lift-archetype-basic -DarchetypeVersion=2.0-M2 -DremoteRepositories=http://scala-tools.org/repo-releases"
    
    

 
