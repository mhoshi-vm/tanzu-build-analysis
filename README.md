# Learning what "tanzu build" does



# Level 1. Very Simple Kubernetes Yaml files

Execute the following 

```
git clone https://github.com/mhoshi-vm/tap-python-recipes
cd tap-python-recipes/py-simple
tanzu apps init
tanzu build config --build-plan-source-type=git --build-plan-source=https://github.com/mhoshi-vm/tanzu-build-playground
tanzu build -o manifests/
```

Examine the outputs

```
mh013301@PJQ72XCV5C python-simple % tree manifests
manifests
└── apps.tanzu.vmware.com.ContainerApp
    └── python-simple
        └── kubernetes-plain
            ├── build
            │   └── sbom
            │       ├── build
            │       │   ├── buildpacksio_lifecycle
            │       │   │   ├── sbom.cdx.json
            │       │   │   ├── sbom.spdx.json
            │       │   │   └── sbom.syft.json
            │       │   ├── paketo-buildpacks_pip
            │       │   │   └── pip
            │       │   │       ├── sbom.cdx.json
            │       │   │       ├── sbom.spdx.json
            │       │   │       └── sbom.syft.json
            │       │   └── sbom.legacy.json
            │       ├── cache
            │       │   ├── paketo-buildpacks_cpython
            │       │   │   └── cpython
            │       │   │       ├── sbom.cdx.json
            │       │   │       ├── sbom.spdx.json
            │       │   │       └── sbom.syft.json
            │       │   ├── paketo-buildpacks_pip
            │       │   │   └── pip
            │       │   │       ├── sbom.cdx.json
            │       │   │       ├── sbom.spdx.json
            │       │   │       └── sbom.syft.json
            │       │   └── paketo-buildpacks_pip-install
            │       │       └── packages
            │       │           ├── sbom.cdx.json
            │       │           ├── sbom.spdx.json
            │       │           └── sbom.syft.json
            │       └── launch
            │           ├── buildpacksio_lifecycle
            │           │   └── launcher
            │           │       ├── sbom.cdx.json
            │           │       ├── sbom.spdx.json
            │           │       └── sbom.syft.json
            │           ├── paketo-buildpacks_ca-certificates
            │           │   └── helper
            │           │       └── sbom.syft.json
            │           ├── paketo-buildpacks_cpython
            │           │   └── cpython
            │           │       ├── sbom.cdx.json
            │           │       ├── sbom.spdx.json
            │           │       └── sbom.syft.json
            │           ├── paketo-buildpacks_pip-install
            │           │   └── packages
            │           │       ├── sbom.cdx.json
            │           │       ├── sbom.spdx.json
            │           │       └── sbom.syft.json
            │           └── sbom.legacy.json
            ├── output
            │   ├── app-config
            │   │   ├── deployment.yml
            │   │   ├── poddisruptionbudget.yml
            │   │   ├── secret-registry.yml
            │   │   ├── service.yml
            │   │   └── serviceaccount.yml
            │   └── containerapp.yml
            └── tanzu.yml

28 directories, 34 files
```

Examine 