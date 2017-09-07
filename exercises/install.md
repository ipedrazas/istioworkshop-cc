# Install Istio

## Prerequisites

You will need a Kubernetes (1.6+) cluster with helm installed.

## Installation

The easiest way to install Istio is, by far, using the helm chart:

Th4e chart is located at the following location: 

[https://github.com/kubernetes/charts/tree/master/incubator/istio]

and it can be installed using the following command:

```
helm init
(wait until tiller is up and running: `kubectl get pods -n kube-system`)
helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
helm install incubator/istio
```

If you have already defined the `incubator` repo, it's good to update it to make sure you have access to the latest charts:

```
helm repo update
```

Istio uses a command line tool `istioctl` to create, list, modify, and delete configuration resources in the Istio system.

To install it, we need to download it, the following command will download the tarball and extracted it.

```
curl -L https://git.io/getIstio | sh -
```

This is the script that it's downloaded and executed by the previous command:


```
    #! /bin/sh
    #
    # Early version of a downloader/installer for Istio
    #
    # This file will be fetched as: curl -L https://git.io/getIstio | sh -
    # so it should be pure bourne shell, not bash
    #
    # The script fetches the latest Istio release and untars it.

    # TODO: Automate updating me.
    ISTIO_VERSION="0.1.6"

    NAME="istio-$ISTIO_VERSION"
    OS="$(uname)"
    if [ "x${OS}" = "xDarwin" ] ; then
    OSEXT="osx"
    else
    # TODO we should check more/complain if not likely to work, etc...
    OSEXT="linux"
    fi
    URL="https://github.com/istio/istio/releases/download/${ISTIO_VERSION}/istio-${ISTIO_VERSION}-${OSEXT}.tar.gz"
    echo "Downloading $NAME from $URL ..."
    curl -L "$URL" | tar xz
    # TODO: change this so the version is in the tgz/directory name (users trying multiple versions)
    echo "Downloaded into $NAME:"
    ls $NAME
    BINDIR="$(cd $NAME/bin; pwd)"
    echo "Add $BINDIR to your path; e.g copy paste in your shell and/or ~/.profile:"
    echo "export PATH=\"\$PATH:$BINDIR\""
```

## Questions

* What's the version of the chart?
* What info provides the version of the chart?
* which components have we deployed?
* How do we verify that all the components are working as expected?
* List the addons installed by this chart
* Use the `port-forwarding` functionality to access them.
* Why do we need a tool like `istioctl`?
* How do you pronounce `istioctl`?
