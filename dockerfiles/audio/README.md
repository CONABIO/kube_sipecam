Set:

```
KALE_VERSION=0.4.0
REPO_URL=palmoreck/jupyterlab_kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/audio/0.4.0
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$KALE_VERSION
```



