# SSAPServer
An implementation of the [Simple Spectral Access Protocol of the VO](http://www.ivoa.net/documents/SSA/20120210/REC-SSA-1.1-20120210.pdf)
The server is a Spring Boot application that implements the API defined in the standard.

The server can either redirect the requests to a TAP service or go directly to the DB.

SSAPServer is at a very early stage of development and currently supports only queries with `POS` and `SIZE`.


## Using TAP
In this mode the server translates the incoming requests into ADQL and sends them to a TAP service.
The tool assumes a specific schema is available on the TAP server. Unfortunately it is not possible to query directly ObsCore, 
because the UCDs are different and some columns are missing. Therefore we defined a view that can be built on top of
ObsCore to enable SSA access. You can find the definition of this view for SQLServer under TBD

## Direct DB queries
Not yet implemented

## Run the server
Execute:
```
./gradlew bootRun -Pargs="--ssap.tap.url=http://<host>:<port>/yourtap"
```
in the project root directory: the server will start at `http://localhost:9000/ssa`
## Build
Execute:
```
./gradlew build
```
in the project root directory, a standalone jar file will be created under `build/libs/SSAPServer-<version>.jar`. You can run it like this:
```
java -jar SSAPServer-<version>.jar --ssap.tap.url=http://<host>:<port>/yourtap
```

## Configuration
Here are all the available configuration options, with their default values. They can be set in any way allowed by Spring Boot (see [here](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html))

```
# Generic configuration
ssap.versions.supported = {1.1} // a list of the supported protocol versions

# Configuration options specific to the TAP access
ssap.use.tap = true // mandatory
ssap.tap.url = // the URL of the TAP server, no default
ssap.tap.timeout = 10 // timeout in seconds

# Spring Boot configuration options
server.port = 9000 // the port where the server runs.
```

