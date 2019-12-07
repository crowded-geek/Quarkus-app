Build the package using:

`mvn package -Pnative -Dquarkus.native.container-build=true`

Then, build the image with:

`docker build -f src/main/docker/Dockerfile.native -t quarkus/getting-started .

Then, run the container using:

`docker run -i --rm -p 8080:8080 quarkus/getting-started`
For deploying to Kubernetes

Run minikube using:

`minikube start`

Then, Instantiate the app using:

`kubectl run quarkus --image=quarkus/getting-started:latest --port=8080 --image-pull-policy=IfNotPresent`

Then, expose the deployment as an internal service using:

`kubectl expose deployment quarkus-quickstart --type=NodePort`

For deploying to OpenShift:

Run minishift using:
`minishift start
`
Build the image on OpenShift
`oc new-build --binary --name=quarkus -l app=quarkus`
`oc patch bc/quarkus -p "{\"spec\":{\"strategy\":{\"dockerStrategy\":{\"dockerfilePath\":\"src/main/docker/Dockerfile.native\"}}}}"`
`oc start-build quarkus --from-dir=. --follow`

To instantiate the image
`oc new-app --image-stream=quarkus:latest`

To create the route
`oc expose service quarkus`

Get the route URL
`export URL="http://$(oc get route | grep quarkus | awk '{print $2}')"`
`echo "Application URL: $URL"`
`curl $URL/hello && echo`