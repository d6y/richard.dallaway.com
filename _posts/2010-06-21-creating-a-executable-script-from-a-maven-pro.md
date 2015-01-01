---
layout: post
comments: true
permalink: /:title
title: 'Creating a executable script from a Maven project'
author: Richard
date: 2010/06/21
alias: /creating-a-executable-script-from-a-maven-pro
tags:
---

If you have a Maven project with a bunch of dependencies, and you've
written a harness to launch your thing, it's handy to be able to create
a shell script to run your harness with all your dependencies on the
classpath.  The Maven assembler plugin is what you need.

In your pom:

    <build>  
      <plugins>    
      ...     
        <plugin>    
          <groupId>org.codehaus.mojo</groupId>    
          <artifactId>appassembler-maven-plugin</artifactId>    
          <configuration>
            <programs>         
              <program>            
                <mainClass>com.example.yourApp.YourClassWithMainMethod</mainClass>
                <name>my_cool_app</name>         
              </program>       
            </programs>     
          </configuration>    
        </plugin>   
      </plugins> 
    </build>

Then run:

    $ mvn package appassembler:assemble

This produces a shell (and .bat) script in `target/appassembler`:
 
    $ sh target/bin/my_cool_app Hello world!

This is one of those "I keep forgetting how to do this so I'd better
blog it so I can find it later" kind of posts: It's not original, but
handy, and I keep forgetting to run the package part of the Maven
command.
