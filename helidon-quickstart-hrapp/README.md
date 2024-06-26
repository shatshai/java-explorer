# Helidon Quickstart HR Application with Helidon Integration (helidon-quickstart-hrapp)

This repository showcases a sample Helidon SE project implementing various REST operations for an HR application (HRApp). It also demonstrates integrating HRApp functionalities with Helidon, potentially a separate system or service.

## Project Highlights:

* Multiple REST Endpoints: The application exposes REST endpoints for managing HR data (functionality details can be added here).
* Helidon Integration: The project showcases interaction with Helidon, likely for enhanced HR functionalities (further details on the integration purpose would be beneficial).

## Getting Started (Optional):

* Provide instructions on cloning the repository, setting up dependencies, and running the application (if applicable).

## Further Exploration (Optional):

* Suggest ways users can modify the project to suit their specific needs (e.g., adding new REST endpoints, customizing Helidon integration).

## Additional Information (Optional):

* Include links to relevant Helidon documentation or resources.

## Build and run

With JDK17+
```bash
mvn package
java -jar target/helidon-quickstart-hrapp.jar
```

## Exercise the application

```
curl -X GET http://localhost:8080/employees
[{"id":101,"name":"Ann","salary":1234.56},{"id":102,"name":"Bob","salary":1200.34},{"id":103,"name":"Ray","salary":1122.33}]

```

## Try metrics

```
# Prometheus Format
curl -s -X GET http://localhost:8080/metrics
# TYPE base:gc_g1_young_generation_count gauge
. . .

# JSON Format
curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
{"base":...
. . .
```



## Try health

```
curl -s -X GET http://localhost:8080/health
{"outcome":"UP",...

```



## Building a Native Image

Make sure you have GraalVM locally installed:

```
$GRAALVM_HOME/bin/native-image --version
```

Build the native image using the native image profile:

```
mvn package -Pnative-image
```

This uses the helidon-maven-plugin to perform the native compilation using your installed copy of GraalVM. It might take a while to complete.
Once it completes start the application using the native executable (no JVM!):

```
./target/helidon-quickstart-hrapp
```

Yep, it starts fast. You can exercise the application’s endpoints as before.


## Building the Docker Image

```
docker build -t helidon-quickstart-hrapp .
```

## Running the Docker Image

```
docker run --rm -p 8080:8080 helidon-quickstart-hrapp:latest
```

Exercise the application as described above.
                                

## Run the application in Kubernetes

If you don’t have access to a Kubernetes cluster, you can [install one](https://helidon.io/docs/latest/#/about/kubernetes) on your desktop.

### Verify connectivity to cluster

```
kubectl cluster-info                        # Verify which cluster
kubectl get pods                            # Verify connectivity to cluster
```

### Deploy the application to Kubernetes

```
kubectl create -f app.yaml                  # Deploy application
kubectl get pods                            # Wait for quickstart pod to be RUNNING
kubectl get service  helidon-quickstart-hrapp         # Get service info
```

Note the PORTs. You can now exercise the application as you did before but use the second
port number (the NodePort) instead of 8080.

After you’re done, cleanup.

```
kubectl delete -f app.yaml
```
                                

## Building a Custom Runtime Image

Build the custom runtime image using the jlink image profile:

```
mvn package -Pjlink-image
```

This uses the helidon-maven-plugin to perform the custom image generation.
After the build completes it will report some statistics about the build including the reduction in image size.

The target/helidon-quickstart-hrapp-jri directory is a self contained custom image of your application. It contains your application,
its runtime dependencies and the JDK modules it depends on. You can start your application using the provide start script:

```
./target/helidon-quickstart-hrapp-jri/bin/start
```

Class Data Sharing (CDS) Archive
Also included in the custom image is a Class Data Sharing (CDS) archive that improves your application’s startup
performance and in-memory footprint. You can learn more about Class Data Sharing in the JDK documentation.

The CDS archive increases your image size to get these performance optimizations. It can be of significant size (tens of MB).
The size of the CDS archive is reported at the end of the build output.

If you’d rather have a smaller image size (with a slightly increased startup time) you can skip the creation of the CDS
archive by executing your build like this:

```
mvn package -Pjlink-image -Djlink.image.addClassDataSharingArchive=false
```

For more information on available configuration options see the helidon-maven-plugin documentation.
                                
