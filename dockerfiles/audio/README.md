Set:

```
KALE_VERSION=0.4.0_10.1_tf_test
REPO_URL=sipecam/audio-kale
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



