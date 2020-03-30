Set:

```
KALE_AND_TF_VERSION=0.4.0_1.14.0_tf
KALE_AND_TF_VERSION=0.4.0_2.1.0
REPO_URL=sipecam/audio-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/dockerfiles
BUILD_DIR=$PATH_DIR_OF_CLONING/audio/tf_kale/0.4.0_1.14.0_tf
BUILD_DIR=$PATH_DIR_OF_CLONING/audio/tf_kale/0.4.0_2.1.0
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$KALE_AND_TF_VERSION
```



