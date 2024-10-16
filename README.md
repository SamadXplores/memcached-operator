# Memcached Operator

## Projektübersicht
Der **Memcached Operator** ist ein Kubernetes-Operator, der die Bereitstellung, Verwaltung und Skalierung von Memcached-Instanzen innerhalb eines Kubernetes-Clusters automatisiert. Er ermöglicht es Entwicklern und Administratoren, Memcached als Cache-Lösung effizient zu nutzen, indem er die Komplexität der manuellen Bereitstellung und Verwaltung reduziert. 

Mit diesem Operator kannst du Memcached-Instanzen durch benutzerdefinierte Ressourcen (Custom Resources) erstellen und verwalten. Der Operator überwacht den Zustand der Memcached-Instanzen und stellt sicher, dass die gewünschte Anzahl von Pods zu jedem Zeitpunkt ausgeführt wird. Bei Bedarf werden neue Instanzen erstellt oder bestehende Instanzen aktualisiert.

## Funktionen

- **Automatische Bereitstellung**: Der Operator kümmert sich um die Bereitstellung und Konfiguration von Memcached-Instanzen.
- **Skalierung**: Unterstützung der Skalierung der Memcached-Instanzen basierend auf Benutzeranforderungen.
- **Überwachung**: Automatisches Überwachen des Status von Memcached-Pods und Wiederherstellen bei Ausfällen.

## Voraussetzungen

- Go Version v1.22.0+
- Docker Version 17.03+
- kubectl Version v1.11.3+
- Zugriff auf ein Kubernetes v1.11.3+ Cluster

## Installation

### 1. Bauen und Pushen des Images

Bauen und pushen Sie Ihr Docker-Image an den im `IMG` angegebenen Speicherort:

```bash
make docker-build docker-push IMG=<some-registry>/memcached-operator:tag

```

**NOTE:** This image ought to be published in the personal registry you specified.
And it is required to have access to pull the image from the working environment.
Make sure you have the proper permission to the registry if the above commands don’t work.

**Install the CRDs into the cluster:**

```sh
make install
```

**Deploy the Manager to the cluster with the image specified by `IMG`:**

```sh
make deploy IMG=<some-registry>/memcached-operator:tag
```

> **NOTE**: If you encounter RBAC errors, you may need to grant yourself cluster-admin
privileges or be logged in as admin.

**Create instances of your solution**
You can apply the samples (examples) from the config/sample:

```sh
kubectl apply -k config/samples/
```

>**NOTE**: Ensure that the samples has default values to test it out.

### To Uninstall
**Delete the instances (CRs) from the cluster:**

```sh
kubectl delete -k config/samples/
```

**Delete the APIs(CRDs) from the cluster:**

```sh
make uninstall
```

**UnDeploy the controller from the cluster:**

```sh
make undeploy
```

## Project Distribution

Following are the steps to build the installer and distribute this project to users.

1. Build the installer for the image built and published in the registry:

```sh
make build-installer IMG=<some-registry>/memcached-operator:tag
```

NOTE: The makefile target mentioned above generates an 'install.yaml'
file in the dist directory. This file contains all the resources built
with Kustomize, which are necessary to install this project without
its dependencies.

2. Using the installer

Users can just run kubectl apply -f <URL for YAML BUNDLE> to install the project, i.e.:

```sh
kubectl apply -f https://raw.githubusercontent.com/<org>/memcached-operator/<tag or branch>/dist/install.yaml
```

## Contributing
// TODO(user): Add detailed information on how you would like others to contribute to this project

**NOTE:** Run `make help` for more information on all potential `make` targets

More information can be found via the [Kubebuilder Documentation](https://book.kubebuilder.io/introduction.html)

## License

Copyright 2024.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

