**Multi Module Java 11**
	
The examples demonstrate migrating a Java multi module project . 

1. Building a multi-module Java 9 project from the command line 
2. Building a multi-module Java 9 project with Maven
	

**1. Building a multi module project from the command line**
	
**1.1. From the project root Java_Module_Demo**
	
  	javac -d mods --module-source-path src 
		src/com.vijfhart.cursus.demo/module-info.java
		src/com.vijfhart.cursus.demo/com/vijfhart/cursus/demo/Demo.java
		src/com.vijfhart.cursus.democlient/module-info.java
		src/com.vijfhart.cursus.democlient/com/vijfhart/cursus/democlient/Main.java
	
**1.2. Create JAR files**
	
  	jar -cf target\demo.jar -C mods\com.vijfhart.cursus.demo .
  	jar -cfe target\democlient.jar com.vijfhart.cursus.democlient.Main -C mods\com.vijfhart.cursus.democlient .
	
**1.3. Execute**
	
  	java --module-path target\demo.jar;target\democlient.jar --module com.vijfhart.cursus.democlient
	
**1.4. Create jmod files**
	
  	jmod create --class-path target\demo.jar jmods\demo.jmod
  	jmod create --class-path target\democlient.jar jmods\democlient.jmod
	
**1.5. Check jmods**

  	jmod describe jmods\demo.jmod
  	jmod describe jmods\democlient.jmod
	
**1.6. Use jlink to link a custom JRE**
	
	jlink
		--module-path "%JAVA_HOME%\jmods;jmods" 
		--add-modules com.vijfhart.cursus.demo,com.vijfhart.cursus.democlient 
		--launcher demo=com.vijfhart.cursus.democlient 
		--output democlient
	
**1.7. Execute**
	
  	democlient\bin\demo
	  

**2. Building a multi-module Java 9 project with Maven** 

**2.2 Use the maven compiler plugin version 3.8.0**

	<properties>
		<maven.compiler.plugin>3.8.0</maven.compiler.plugin>
		<maven.compiler.source>1.11</maven.compiler.source>
		<maven.compiler.target>11</maven.compiler.target>
		<maven.compiler.release>11</maven.compiler.release>
	</properties>

Older versions of the Maven compiler plugin use an old version of asm.jar incompatible with Java10 or 11. To force a newer version, add this dependency to the compiler plugin.

	<plugin>
  	  <groupId>org.apache.maven.plugins</groupId>
  	  <artifactId>maven-compiler-plugin</artifactId>
  	  <dependencies>
    	    <dependency>
      	      <groupId>org.ow2.asm</groupId>
      	      <artifactId>asm</artifactId>
      	      <version>6.2</version>
    	    </dependency>
  	  </dependencies>
	</plugin>


**2.4. From the project root Java_Module_Maven_Demo**

  	mvn clean install package

**2.5. Execute**

  	java -p target\jars -m com.vijfhart.cursus.democlient/com.vijfhart.cursus.democlient.Main
	 
