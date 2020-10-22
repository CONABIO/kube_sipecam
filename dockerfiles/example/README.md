Set:

```
KALE_EXAMPLE=0.5.0
REPO_URL=sipecam/example-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/example/0.5.0
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$KALE_EXAMPLE
```

Check:

```
docker run --rm -v $(pwd):/datos --name sipecam-example -p 8888:8888 $REPO_URL:$KALE_EXAMPLE
```
