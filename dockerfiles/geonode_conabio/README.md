Set:

```
GEONODE_CONABIO_KALE=0.1_0.5.0
REPO_URL=sipecam/geonode-conabio-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/geonode_conabio/$GEONODE_CONABIO_KALE
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$GEONODE_CONABIO_KALE
```

Check:

```
docker run --rm -d -v $(pwd):/datos --name sipecam-geonode-conabio -p 8888:8888 $REPO_URL:$GEONODE_CONABIO_KALE
```
