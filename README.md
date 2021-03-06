fabric8-persistence-demo
======================

Fabric8 + E-OSGi JPA 2.1 managed persistence (Aries + Hibernate 4.3.x) + REST service demonstration code.

# Pre-requisites

* JDK 7
* Maven 3.1.0 or newer

# Build and install

```
mvn clean install
```

# Provisioning

## Installation and initial configuration

* Download [latest build](https://repository.jboss.org/nexus/content/repositories/ea/io/fabric8/fabric8-karaf/) for ```fabric-karaf``` and extract it.
*(tested on fabric8-karaf-1.2.0.Beta3)*
* Extract it
* Start Fabric
```no-highlight
bin/fabric8
```

If everything goes well, you should get a Fabric shell that looks like this:

```
Please wait while Fabric8 is loading...
100% [========================================================================]

______    _          _      _____
|  ___|  | |        (_)    |  _  |
| |_ __ _| |__  _ __ _  ___ \ V /
|  _/ _` | '_ \| '__| |/ __|/ _ \
| || (_| | |_) | |  | | (__| |_| |
\_| \__,_|_.__/|_|  |_|\___\_____/
  Fabric8 Container (1.2.0.Beta3)
  http://fabric8.io/

Type 'help' to get started
and 'help [cmd]' for help on a specific command.
Hit '<ctrl-d>' or 'osgi:shutdown' to shutdown this container.

Open a browser to http://localhost:8181 to access the management console
```

## Define our own profile
```
profile-create --parents quickstarts-karaf-cxf-rest persistence-example
profile-edit --repositories mvn:com.github.pires.example/feature-persistence/0.1-SNAPSHOT/xml/features persistence-example
profile-edit --features persistence-aries-hibernate persistence-example
```

## Create and run new container with newly created profile

```
container-create-child --profile persistence-example root test
```

# Testing

In Hawt.io UI, go to ```API``` tab (in the parent container), check the host and port where ```UserService``` is available and point it down. Test the REST endpoint as you wish!

## REST API (JSON)

Create new user
```
PUT /user

Example JSON:
{
  "name":"Pires"
}
```

Count users
```
GET /user/count
```

List users
```
GET /user
```

### curl commands
```
curl -H "Accept: application/json" -H "Content-type: application/json" -X PUT -d '{"name":"Pires"}' http://localhost:8182/cxf/demo/user ; echo

curl -H "Accept: application/json" -H "Content-type: application/json" -X GET  http://localhost:8182/cxf/demo/user/count ; echo
```

# Developing

If you are changing these bundles remember you can use the ``fabric:watch *`` command.  Fabric will automatically update bundles that change after a ``mvn install``
