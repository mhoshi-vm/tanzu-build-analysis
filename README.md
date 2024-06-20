# Learning what "tanzu build" does

```
git clone https://github.com/mhoshi-vm/tanzu-build-analysis
cd tanzu-build-analysis
git clone https://github.com/mhoshi-vm/tap-python-recipes
cd tap-python-recipes/py-simple
tanzu apps init
```

# Level 1. Very Simple Kubernetes Yaml files

Execute the following 

```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level1/simple.yaml
tanzu build -o manifests/
```

Examine the outputs

```
mh013301@PJQ72XCV5C python-simple % tree manifests
manifests
└── apps.tanzu.vmware.com.ContainerApp
    └── python-simple
        └── plain
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

# Level 2. Switch between App Runtimes

If multiple runtimes are defined can be switched via `-r` and specifying the name

Trying plain
```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level2/simple.yaml
tanzu build -o manifests/ -r plain
```

Trying image
```
python-simple % tree manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/plain/output 
manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/plain/output
├── app-config
│   ├── deployment.yml
│   ├── poddisruptionbudget.yml
│   ├── secret-registry.yml
│   ├── service.yml
│   └── serviceaccount.yml
└── containerapp.yml
```

```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level2/simple.yaml
tanzu build -o manifests/ -r image
```

```
python-simple % tree manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/image/output
manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/image/output
├── containerapp.yml
└── image

1 directory, 2 files
```

# Level 3. Understanding postBuildSteps

```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level3/simple.yaml
tanzu build -o manifests/ 
```

```
python-simple % tree manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output 
manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output
├── app-config
│   ├── deployment.yml
│   ├── poddisruptionbudget.yml
│   ├── secret-registry.yml
│   ├── service.yml
│   └── serviceaccount.yml
└── containerapp.yml
```
Diff against runtime (plain) but no major diff other than meta info
```
python-simple % diff -r manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/plain/output
diff -r manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output/app-config/deployment.yml manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/plain/output/app-config/deployment.yml
19c19
<         containerapp.apps.tanzu.vmware.com/content-summary: git:8183b20 @ 2024-06-20T11:58:57Z
---
>         containerapp.apps.tanzu.vmware.com/content-summary: git:8183b20 @ 2024-06-20T11:48:23Z
26c26
<       - image: ttl.sh/tp-saas@sha256:c3a084c2cb11edcae5c1e7b99d8353c6a1ea616d76511e039b8a271e1744c9cd
---
>       - image: ttl.sh/tp-saas@sha256:9d76e2315c8feb55e5bc6582aca4b7242810816467aec01ab41e309e138cedd9
diff -r manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output/containerapp.yml manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/plain/output/containerapp.yml
10c10
<     buildTimestamp: "2024-06-20T11:58:57Z"
---
>     buildTimestamp: "2024-06-20T11:48:23Z"
13,15c13,15
<     summary: git:8183b20 @ 2024-06-20T11:58:57Z
<     version: 20240620.1158.57617
<   image: ttl.sh/tp-saas@sha256:c3a084c2cb11edcae5c1e7b99d8353c6a1ea616d76511e039b8a271e1744c9cd
---
>     summary: git:8183b20 @ 2024-06-20T11:48:23Z
>     version: 20240620.1148.23660
>   image: ttl.sh/tp-saas@sha256:9d76e2315c8feb55e5bc6582aca4b7242810816467aec01ab41e309e138cedd9
```

As of today `postBuildSteps` and `runtimes` have no difference but `postBuildSteps` cannot be controled where as `runtimes` can be controlled via `-r`

# Level 4. NamedTask

Named task as of today is hard coded in side of `tanzu build` plugin.  
[Only accessible from BC employees](https://gitlab.eng.vmware.com/build-service/tanzu-build-plugin/-/blob/main/pkg/namedtask/namedtask.go#L37)

# Level 5. ContainerTask

Currently, to customize the output we can use `containertasks`

```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level5/simple.yaml
tanzu build -o manifests/ 
```

During build we can see the some variables and the contents are accessible via the container task image

```
👌 Successfully built 'ttl.sh/tp-saas@sha256:ac0f312e5c96e717cdf7903f913538b7656e5c224ad73952f0f2eb425f7a55df'
🏃 Running post build
running step 
latest: Pulling from library/alpine
Digest: sha256:77726ef6b57ddf65bb551896826ec38bc3e53f75cdde31354fbffb4f25238ebd
Status: Image is up to date for alpine:latest
HOSTNAME=fb730e202aef
SHLVL=1
HOME=/root
TANZU_BUILD_APP_RUNTIME=
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
TANZU_BUILD_TARGET=release
TANZU_BUILD_WORKSPACE_DIR=/workspace
PWD=/workspace
total 12
drwxr-x---    3 root     root          4096 Jun 20 12:00 build
drwxr-x---    3 root     root          4096 Jun 20 12:00 output
-rw-r--r--    1 root     root           106 Jun 20 12:00 tanzu.yml
📝 Wrote output to 'manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/output'
```

# Level 6. Adding YAML with ContainerTask

```
tanzu build config --build-plan-source-type=file --build-plan-source=../../level6/simple.yaml       
tanzu build -o manifests  
```

The yaml can now be added to the output

```
python-simple % cat ./manifests/apps.tanzu.vmware.com.ContainerApp/python-simple/default/workspace/output/dummy.yaml
foo: bar
```