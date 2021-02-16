Set:

```
REPO_URL=sipecam/audio-dgpi-kale-tensorflow-gpu
VERSION=0.6.1
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/audio/tensorflow-gpu/$VERSION/
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$VERSION
```

Check:

```
docker run --rm -d -v $(pwd):/datos --name sipecam-audio-dgpi -p 8888:8888 $REPO_URL:$VERSION
```


