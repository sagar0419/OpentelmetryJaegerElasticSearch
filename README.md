## AUtomatic instrumentation can be enabled by adding the operator to your cluster and than adding the instrumentation annotation to the Deployment, Statefulset, Namespace or other resources.

1. Log on to your Kubernetes cluster.

2. Install cert-manager. From this page 'https://cert-manager.io/docs/installation/#default-static-install'
   `kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.8.2/cert-manager.yaml`

3. Install Jaeger Operator. 'https://www.jaegertracing.io/docs/1.35/operator/'
   'kubectl create namespace observability'
   'kubectl create -f https://github.com/jaegertracing/jaeger-operator/releases/download/v1.35.0/jaeger-operator.yaml -n observability'

4. Create opentelemetry operrator on Kubernetes. You can follow the official github repo of opentelemetery (https://github.com/open-telemetry/opentelemetry-operator).
   `kubectl apply -f opentelemetry-operator.yaml`

5. create the auto instrumentation on the namespace where you want to deploy your application.
   `kubectl apply -f `instrumentation.yaml`

6. Create sample app.
   'kubectl apply -f sample-app.yaml'
#change the value in line 37 according to your code.
The annotation can be added to a namespace, so that all pods within that namespace wil get instrumentation, or by adding the annotation to individual PodSpec objects, available as part of Deployment, Statefulset, and other resources.

Java:
`instrumentation.opentelemetry.io/inject-java: "true"`

NodeJS:
`instrumentation.opentelemetry.io/inject-nodejs: "true"`

Python:
`instrumentation.opentelemetry.io/inject-python: "true"`


7. Create configmap for elasticsearch.
   `kubectl apply -f elasticsearch-config.yaml`

#Change username and password according to your need,

8. Create elasticsearch deployment.
   `kubectl apply -f elasticsearch.yml`

#Jaeger Tracing updated file for backend storage can be found from "https://github.com/jaegertracing/jaeger-kubernetes"

9. Installl kibana on the server for the visualisation of the data.
   `kubectll apply -f kibana.yaml`

#You need to change the values of "ELASTICSEARCH_URL" , "ELASTICSEARCH_USER" , "ELASTICSEARCH_PASSWORD" depends upon you deployment. Use username and password which you have mentioned in elasticsearch-config.yaml file.
