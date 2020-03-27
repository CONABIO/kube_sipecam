Set:

```
TFX_VERSION=1.14.0_tfx
REPO_URL=sipecam/audio-tfx
PATH_DIR_OF_CLONING=/home/<user>/<midir>/dockerfiles
BUILD_DIR=$PATH_DIR_OF_CLONING/audio/tf/tfx/1.14.0
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$TFX_VERSION
```



