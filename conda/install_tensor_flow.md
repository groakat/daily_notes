# Installing tensorflow

## Problem

Following the installation guide of the tensor-flow website, leads to the following error:

    Cannot remove entries from nonexistent file /Users/peter/anaconda/envs/thirdeye/lib/python3.5/site-packages/easy-install.pth

## Solution

As described in https://github.com/tensorflow/tensorflow/issues/622 use the

    --ignore-installed

flag.

Complete install with:

    export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/tensorflow-0.9.0-py3-none-any.whl
    pip install --upgrade $TF_BINARY_URL --ignore-installed

