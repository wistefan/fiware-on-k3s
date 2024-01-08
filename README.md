# fiware-on-k3s

Example setup of the [FIWARE Orion-LD ContextBroker](https://github.com/FIWARE/context.Orion-LD) in a local kubernetes, provided via [k3s](https://k3s.io/).
The setup is configured close to the GitOps approach, used in [FIWARE-Ops/fiware-gitops](https://github.com/FIWARE-Ops/fiware-gitops) or [FIWARE-Ops/marinera](https://github.com/FIWARE-Ops/marinera).
Compnents are defined via [helm-charts](charts), then the charts get rendered to kubernetes manifests and applied to the k3s cluster.
For easy usage, all this is provided via maven:
- copy to the target folder - [maven-resources-plugin](https://maven.apache.org/plugins/maven-resources-plugin/)
- build and template the helm-charts - [helm-maven-plugin](https://github.com/kokuwaio/helm-maven-plugin)
- run k3s and apply the manifests - [k3s-maven-plugin](https://github.com/kokuwaio/k3s-maven-plugin)

The rendered templates can be found under the ```target/k3s``` folder. 

## Run
> To use this project, [maven](https://maven.apache.org/install.html) needs to be installed first.

The plugins are usually used in test-environments, therefore they are bound to the test-phases inside the [maven-lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html):
- ```validate``` - copy the charts 
- ```test-compile``` - build and template the charts
- ```pre-integration-test``` - start k3s and install the components

To run the full setup, use ```mvn clean integration-test```

## Access the services

The followijng services are available at localhost after running the setup:
- [Orion-LD](https://github.com/FIWARE/context.Orion-LD) and the [NGSI-LD Api](https://docbox.etsi.org/isg/cim/open/Latest%20release%20NGSI-LD%20API%20for%20public%20comment.pdf) at ```http://localhost:1026```
- [Grafana](https://grafana.com/) at ```http://localhost:3000```, username: fiwareAdmin & password: fiwareAdmin
- [Kong](https://konghq.com) at ```http://localhost:6060``` as secured entrypoint to the broker
- [Keycloak](https://www.keycloak.org/) at ```http://localhost:70707``` as the idm to be used with kong

## Stopping it

To stop and remove the setup and clean the whole workspace, use ```mvn clean k3s:stop k3s:rm```.