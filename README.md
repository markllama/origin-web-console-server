OpenShift Web Console Server
============================

The API server for the [OpenShift web console](https://github.com/openshift/origin-web-console), part of the
[OpenShift](https://github.com/openshift/origin) application platform.

[![Build Status](https://travis-ci.org/openshift/origin-web-console-server.svg?branch=master)](https://travis-ci.org/openshift/origin-web-console-server)

The web console server runs as a pod on the platform. The OpenShift master
proxies requests from the web console context root, typically `/console/`, to
the server running in the pod. The pod then serves the static HTML, JavaScript,
and CSS files that make up the console.

The web console assets themselves are developed in the
[origin-web-console](https://github.com/openshift/origin-web-console)
repository. They are included in the web console server binary using
[go-bindata](https://github.com/jteeuwen/go-bindata).

Building
--------

To build the binary, run

```
$ make
```

To build the RPM and images, run

```
$ make build-images
```

If you are running on a non-Linux platform, you can build the images in a
container with this command

```
$ OS_BUILD_ENV_PRESERVE=_output/local/bin hack/env make build-images
```

Installing the Console
----------------------

If you use [openshift-ansible](https://github.com/openshift/openshift-ansible)
or run `oc cluster up`, the console will be installed for you. If you start
OpenShift another way, you'll need to install the console template.

Clone the [origin](https://github.com/openshift/origin) repository and
edit the file `install/origin-web-console/console-config.yaml` for your
cluster. Then run the commands:

```
$ oc login -u system:admin
$ oc create namespace openshift-web-console
$ oc process -f install/origin-web-console/console-template.yaml -p "API_SERVER_CONFIG=$(cat install/origin-web-console/console-config.yaml)" | oc apply -n openshift-web-console -f -
```

Updating Go Tooling
-------------------

See https://github.com/openshift/release/tree/master/tools/hack/golang for
instructions on how to update the Go tooling used by this project.
