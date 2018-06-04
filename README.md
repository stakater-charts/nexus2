# chart-nexus
This repository contains 2 charts that are used to deploy nexus to kubernetes.
- nexus-storage
- nexus

## Installing
First install `nexus-storage` chart
```
helm install --name nexus2-storage chartmuseum/nexus2-storage
```

After that, install `nexus` chart
```
helm install --name nexus2 chartmuseum/nexus2
```
