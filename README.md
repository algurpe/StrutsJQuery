Step1: Install and Configure Maven
We use Maven to describe this software project and to manage the dependencies.

1.- Download maven from http://maven.apache.org/download.html and extract it.

2.- Add MAVEN_HOME/bin to your PATH
Step2: Create your Struts2 project

1.- Switch to your Eclipse Workspace.
`cd workspace`

2.- Create the project based on the struts2-archetype-convention archetype.

```
mvn archetype:generate -B -DgroupId=com.jgeppert.examples -DartifactId=struts2-example -DarchetypeGroupId=org.apache.struts -DarchetypeArtifactId=struts2-archetype-convention -DarchetypeVersion=2.3.24
```

3.- Maven downloads all needed ...

4.- Go into the new created folder struts2-example.
`cd struts2-example`

5.- Open `pom.xml` in an editor.

6.- Your project version is set to `1.0-SNAPSHOT`
`<version>1.0-SNAPSHOT</version>`

7.- Be sure your Struts2 version in the pom.xml properties section is set to 2.3.24 or above.

```
    <properties>
        <struts2.version>2.3.24</struts2.version>
    </properties>
```

8.- Build your _blank application_.
`mvn install`

9.- With the integrated maven jetty plugin you can run your web application and try it out.
`mvn jetty:run`

10.- Open `http://localhost:8080/struts2-example/`` in your Browser
The Message "Struts is up and running..." should appear.

Step3: Create the Eclipse Project

1.- Open your Eclipse IDE.

2.- Install m2-eclipse and restart your IDE.

3.- Open the Import Dialog in your Eclipse IDE and import your created Project `struts2-example`.

Step4: Add the Struts2 jQuery Plugin to your Project

The Struts2 jQuery Plugin provides an easy Integration of jQuery into this Project.

1.- Open your `pom.xml` and edit your project properties to add the version of the Struts2 jQuery plugin.

```    <properties>
        <struts2.version>2.3.24</struts2.version>
        <struts2jquery.version>3.7.1</struts2jquery.version>
    </properties>
```

2.- Add the dependency for the Struts2 jQuery plugin

```
	<dependency>
		<groupId>com.jgeppert.struts2.jquery</groupId>
		<artifactId>struts2-jquery-plugin</artifactId>
		<version>${struts2jquery.version}</version>
	</dependency>
```

3.- Create a new Action Class `YourNameAction.java` inside of the `com.jgeppert.examples.actions` package.

```
package com.jgeppert.examples.actions;
import com.opensymphony.xwork2.ActionSupport;

public class YourNameAction extends ActionSupport {

    private String name;

    public String execute() throws Exception {
        if(name == null || name.length() < 3){
            addActionError("Please enter valid name with more the 2 characters!");
            return ERROR;
	    }

        return SUCCESS;
    }

    public String getName() {
        return name;
    }


    public void setName(String name) {
        this.name = name;
    }
}
```

4.- Replace the content of `hello.jsp`

```
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<%@ taglib prefix="sj" uri="/struts-jquery-tags"%>
<html>
<head>
    <title>My App</title>
    <sj:head jquerytheme="start"/>
    <style>
    	body {
    		font-family: Arial,sans-serif;
    		font-size: 9pt;
    	}
    </style>
</head>

<body>
	<h2>Please enter a Name</h2>

	<s:form action="your-name" theme="xhtml">
		<s:textfield name="name" label="Enter your Name"/>
		<sj:submit
			targets="result"
			effect="highlight"
			value="Submit"
			button="true"
		/>
	</s:form>

	<h3>AJAX Result</h3>
	<div id="result"></div>
</body>
</html>
```

5.- Create a new JSP inside of your `WEB-INF/content` folder called `your-name.jsp`.

```
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<h4>Welcome <s:property value="name"/>!</h4>
```

6.- Create a new JSP inside of your `WEB-INF/content` folder called `your-name-error.jsp`.

```
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="s" uri="/struts-tags" %>
<s:actionerror theme="jquery"/>
```

7.- Run `mvn install` and `mvn jetty:run`.

8.- Open `http://localhost:8080/struts2-example/`` in your browser and our created form should be appear.

9.- Submit without enter a name. The error message should be visible inside of our defined target div without reloading the whole Page.

10.- Submit you form with an valid name.
