# Grails War APP

## Prerequisites

General:

* Grails 3.3.8
* Groovy 2.4.15

## Run the app

To run the app with docker you need to copy `docker-compose.override.dist.yml` to `docker-compose.override.yml` 
and set up the custom configuration. Follow the instructions in the file for more details. 
Then within the project directory run 

`docker-compose up` 

Docker will build the war file using the `grails.env` that has been specified in the `docker-compose.override.yml` file 
by the `APP_ENV` environment variable.
If you have set the `RUN_APP` environment variable  equal to `true` in the `docker-compose.override.yml` then docker will 
run tomcat and the app will be accessible at  

`http://localhost:8080/grails/app/`

## The bug 

Even though grails builds the war file using the right environment when you access the app the 
`Environment.current.name` value is set to `development`

To replicate:
Run `docker-compose up` with: 

```
RUN_APP=true
APP_ENV=staging
```

If you access the app on browser, you should get the default grails json response and it will look something like
```
{
  "message": "Welcome to Grails!",
  "environment": "development",
  "appversion": "0.1",
  "grailsversion": "3.3.8",
  "appprofile": "rest-api",
  "groovyversion": "2.4.15",
  "jvmversion": "1.8.0_171",
  "reloadingagentenabled": false,
  "artefacts": {
    "controllers": 1,
    "domains": 0,
    "services": 0
  }
  ...
  ...
}
```

### Expected behaviour:
You should see the environment value in the json response set to staging 

### Actual behaviour:
Environment value is development
