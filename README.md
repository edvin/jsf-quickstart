# jsf-quickstart

## JSF Quickstart Maven archetype with libsass-filter

This Maven archetype generates a webapp configured with the following goodness:

 - JSF Configuration
 - libsass-filter for on-the-fly SASS to CSS compilation
 - Rewrite-servlet with pretty-faces for SEO urls
 - WildFly deployment configured, app mounts at /
 - Production and Development mode
    
### Create a new project

    mvn archetype:generate \
    -DarchetypeGroupId=no.tornado \
    -DarchetypeArtifactId=jsf-quickstart-archetype \
    -DarchetypeVersion=0.9.1 \
    -DgroupId=com.example \
    -DartifactId=myapp \
    -Dpackage=myapp \
    -Dversion=1.0-SNAPSHOT

The generated folder structure will look like this:

    myapp/pom.xml
    myapp/src
    myapp/src/main
    myapp/src/main/java
    myapp/src/main/java/myapp
    myapp/src/main/resources
    myapp/src/main/webapp
    myapp/src/main/webapp/index.xhtml
    myapp/src/main/webapp/resources
    myapp/src/main/webapp/resources/script
    myapp/src/main/webapp/resources/script/app.js
    myapp/src/main/webapp/resources/style
    myapp/src/main/webapp/resources/style/app.scss
    myapp/src/main/webapp/template.xhtml
    myapp/src/main/webapp/WEB-INF
    myapp/src/main/webapp/WEB-INF/faces-config.xml
    myapp/src/main/webapp/WEB-INF/jboss-web.xml
    myapp/src/main/webapp/WEB-INF/pretty-config.xml
    myapp/src/main/webapp/WEB-INF/web.xml
    
The deployment descriptor `pom.xml` is preconfigured with two profiles:

#### Profile 'development' (default)
    
- JSF project stage is set to `Development`
- libsass-filter recompiles stylsheets on each request
- Separate Wildfly deployment configuration
     
#### Profile 'production'
    
- JSF project stage is set to `Production`
- libsass-filter caches stylsheets after initial request
- Separate Wildfly deployment configuration

### Going live

Activate the production profile to push your project to production:

    mvn clean wildfly:deploy

### Wildfly configuration

To configure a new Wildfly installation you need to add an administration user.

Add administration user to Wildfly:

    $WILDFLY_HOME/bin/add-user.sh admin secret

Then configure your `pom.xml` to match:

    <profile>
        <id>production</id>
        <properties>
            <jsfProjectStage>Production</jsfProjectStage>
            <libsassCache>true</libsassCache>
            <libsassOutputStyle>compressed</libsassOutputStyle>
            <wildflyHostname>my-server-hostname</wildflyHostname>
            <wildflyPort>9990</wildflyPort>
            <wildflyUsername>admin</wildflyUsername>
            <wildflyPassword>secret</wildflyPassword>
        </properties>
    </profile>

### Custom app mountpoint

To mount your url at something other than /, update the `context-root` parameter in
`jboss-web.xml` or delete the file to deploy to the finalName Maven generates.