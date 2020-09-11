Set:

```
REPO_URL=sipecam/rpy2-kale
VERSION=0.5.0
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/rpy2/$VERSION/
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:VERSION
```

Check:

```
docker run --rm -d -v $(pwd):/datos --name sipecam-rpy2 -p 8888:8888 $REPO_URL:$VERSION
```


