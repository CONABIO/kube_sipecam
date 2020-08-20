Set:

```
MAD_MEX_ODC_AND_KALE_VERSION=0.1.0_1.8.3_0.5.0
REPO_URL=sipecam/madmex-odc-kale
PATH_DIR_OF_CLONING=/home/<user>/<midir>/kube_sipecam
BUILD_DIR=$PATH_DIR_OF_CLONING/dockerfiles/MAD_Mex/odc_kale/$MAD_MEX_ODC_AND_KALE_VERSION
```

Clone:

```
git clone https://github.com/CONABIO/kube_sipecam.git $PATH_DIR_OF_CLONING
```

Build:

```
docker build $BUILD_DIR --force-rm -t $REPO_URL:$MAD_MEX_ODC_AND_KALE_VERSION
```

Check:

```
docker run --rm -d -v $(pwd):/datos --name sipecam-madmex -p 9999:8888 $REPO_URL:$MAD_MEX_ODC_AND_KALE_VERSION
```
