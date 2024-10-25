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

**HINWEIS:** Dieses Image sollte in dem angegebenen persönlichen Registry veröffentlicht werden. Es ist erforderlich, Zugriff zu haben, um das Image aus der Arbeitsumgebung abrufen zu können. Stelle sicher, dass du die erforderlichen Berechtigungen für das Registry hast, falls die obigen Befehle nicht funktionieren.

**Installiere die CRDs im Cluster:**

```sh
make install
```

**Deploye den Manager im Cluster mit dem im `IMG`: angegebenen Image**

```sh
make deploy IMG=<some-registry>/memcached-operator:tag
```

> **HINWEIS:** Solltest du auf RBAC-Fehler stoßen, musst du dir möglicherweise Cluster-Admin-Berechtigungen erteilen oder als Admin angemeldet sein.

**Erstelle Instanzen deiner Lösung**

Du kannst die Beispiele aus dem Verzeichnis config/sample anwenden:

```sh
kubectl apply -k config/samples/
```

>**HINWEIS:** Stelle sicher, dass die Beispiele Standardwerte enthalten, um sie testen zu können.

### Deinstallation

**Lösche die Instanzen (CRs) aus dem Cluster:**

```sh
kubectl delete -k config/samples/
```

**Lösche die APIs (CRDs) aus dem Cluster:**

```sh
make uninstall
```

**Entferne den Controller aus dem Cluster:**

```sh
make undeploy
```

## Projektverteilung

Die folgenden Schritte sind erforderlich, um den Installer zu bauen und dieses Projekt an Nutzer zu verteilen.

Baue den Installer für das Image, das im Registry gebaut und veröffentlicht wurde:

```sh
make build-installer IMG=<some-registry>/memcached-operator:tag
```

HINWEIS: Das oben erwähnte makefile-Ziel erzeugt eine install.yaml-Datei im dist-Verzeichnis. Diese Datei enthält alle mit Kustomize erstellten Ressourcen, die zur Installation dieses Projekts ohne Abhängigkeiten erforderlich sind.


2. Nutzung des Installers

Nutzer können das Projekt einfach installieren, indem sie kubectl apply -f <URL für das YAML-BUNDLE> ausführen, z. B.:

```sh
kubectl apply -f https://raw.githubusercontent.com/<org>/memcached-operator/<tag or branch>/dist/install.yaml
```

## Beitrag leisten

// TODO(benutzer): Füge detaillierte Informationen hinzu, wie andere zu diesem Projekt beitragen können.

**HINWEIS:** Führe `make help` aus, um mehr über alle möglichen `make`-Ziele zu erfahren

Mehr Informationen sind in der [Kubebuilder Documentation] zu finden (https://book.kubebuilder.io/introduction.html)

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

