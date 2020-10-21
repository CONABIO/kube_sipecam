Set:

```
ECOINF_AND_KALE=0.5.1
REPO_URL=sipecam/ecoinf-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/ecoinf/0.5.1
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$ECOINF_AND_KALE
```

Check:

```
docker run -dit --rm -v $(pwd):/datos --name sipecam-ecoinf-kale -p 8888:8888 $REPO_URL:$ECOINF_AND_KALE
```
