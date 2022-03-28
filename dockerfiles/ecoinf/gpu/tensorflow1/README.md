Set:

```
ECOINF_AND_KALE_GPU=0.6.1_2
REPO_URL=sipecam/ecoinf-tensorflow1-kale-gpu
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/ecoinf/gpu/tensorflow1/0.6.1_2
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$ECOINF_AND_KALE_GPU
```

Check:

```
docker run -dit --rm -v $(pwd):/datos --name sipecam-ecoinf-kale-gpu -p 8888:8888 $REPO_URL:$ECOINF_AND_KALE_GPU
```
