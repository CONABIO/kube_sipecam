Set:

```
KALE_AND_MAD_MEX_VERSION=0.5.0_0.1.0
REPO_URL=sipecam/madmex-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/MAD_Mex/kale_geopython/0.5.0
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$KALE_AND_MAD_MEX_VERSION
```

Check:

```
docker run --rm -v $(pwd):/datos --name sipecam-madmex -p 9999:8888 sipecam/madmex-kale:0.5.0_0.1.0
```
