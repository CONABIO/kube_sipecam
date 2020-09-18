Set:

```
TORCH_AND_KALE_VERSION=1.4.0_0.5.0
REPO_URL=sipecam/torch-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/torch/$TORCH_AND_KALE_VERSION
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$TORCH_AND_KALE_VERSION
```

Check:

```
docker run --rm -d -v $(pwd):/datos --name sipecam-torch -p 8888:8888 $REPO_URL:$TORCH_AND_KALE_VERSION
```
