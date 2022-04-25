[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Discord Chat](https://img.shields.io/badge/chat-discord-7289da)](https://discord.gg/gzKFYnpmCY)

WildFly Bootable Faces
==========================

### Goals
***
The main goal was to take an out of the box JSF application ([PrimeFaces Showcase](https://github.com/primefaces/primefaces-showcase)) 
and run it in both a Java EE Server and in [WildFly Bootable](https://docs.wildfly.org/bootablejar/). 
Some addition goals:
- See how much we can improve performance by incorporating various optimization tricks for JSF applications
- See if WildFly Bootable is a viable option for JSF and migrating to Docker containers

### Environment
***
- OpenJDK 11.0.10
- JBoss Wildfly 18.0.1
- WildFly Bootable 26.0
- JSF Production Mode
- Intel(R) Core(TM) i7-8750H CPU @2.21 GHz 16GB RAM

### Optimizations
***
- Apache MyFaces (Bootable) instead of Jakarta Mojarra (Wildfly)
- PrimeFaces [MOVE_SCRIPTS_TO_BOTTOM](https://primefaces.github.io/primefaces/10_0_0/#/gettingstarted/configuration?id=configuration)
- OmniFaces [GzipResponseFilter](https://showcase.omnifaces.org/filters/GzipResponseFilter)
- OmniFaces [CombinedResourceHandler](https://showcase.omnifaces.org/resourcehandlers/CombinedResourceHandler)
- PrimeFaces Extensions [CombinedResourceHandler Helper](https://github.com/primefaces-extensions/primefaces-extensions/issues/293) 
- PrimeFaces Extensions [Optimizer](https://github.com/primefaces-extensions/resources-optimizer-maven-plugin)
- jQuery [Hide Page Until Complete](https://stackoverflow.com/questions/9550760/hide-page-until-everything-is-loaded-advanced/28129691#28129691)

### Metrics
***
The following client and server metrics were captured while hitting the exact same page [/datatable/crud.xhtml](https://www.primefaces.org/showcase/ui/data/datatable/crud.xhtml)
Using `Incognito Mode` and pressing CTRL+F5 so it forced the browser to load all resources from the server with nothing cached.

Metric                |  WildFly EE | Bootable       | Bootable (optimized) | Improvement |
----------------------| ----------  | ---------------| --------------------|-------------|
Package Size          | 48.5 MB WAR | 154.0 MB JAR   | 154.0 MB JAR        | -217.53%    |
Cold Startup          | 10.3 s      | 8.7 s          | 8.7 s               | 15.53%      |
Memory Used           | 140 MB      | 66.8 MB        | 66.8 MB             | 52.21%      |
HTTP Requests         | 114         | 114            | 89                  | 21.93%      |
Resource Size         | 4.4 MB      | 4.4 MB         | 4.4 MB              | -----       |
Transferred Size      | 4.4 MB      | 4.4 MB         | 2.9 MB              | 34.09%      |
DOM Loaded            | 1150 ms     | 1340 ms        | 806 ms              | 29.91%      |
Lighthouse Score      | 59/100      | 51/100         | 98/100              | 66.10%      |
First Paint           | 2.4 s       | 2.6 s          | 0.5 s               | 79.17%      |
Largest Paint         | 2.7 s       | 4.2 s          | 1.1 s               | 73.81%      |
Speed Index           | 2.4 s       | 2.6 s          | 0.9 s               | 62.50%      |
Time To Interactive   | 3.9 s       | 4.0 s          | 1.2 s               | 69.23%      |


### Development

***
To run the example in Dev mode:

```
git clone https://github.com/melloware/wildfly-faces
cd wildfly-faces
mvn clean wildfly-jar:dev-watch
```

Then open your web browser to http://localhost:8080/

### Production

***
To run the example in HotSpot Production mode (GraalVM native-image not supported):

```
git clone https://github.com/melloware/wildfly-faces
cd wildfly-faces
mvn clean package -Pprod
java -jar target/wildfly-faces-bootable.jar
```

Then open your web browser to http://localhost:8080/


### Docker

***
```
mvn clean package
docker build -f docker/Dockerfile -t melloware/wildfly-faces . 
docker run -i --rm -p 8080:8080 melloware/wildfly-faces
```

OR run already pushed image:
```
docker run -i --rm -p 8080:8080 melloware/wildfly-faces
