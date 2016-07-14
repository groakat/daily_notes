# CUDNN_STATUS_INTERNAL_ERROR and CUDNN_STATUS_BAD_PARAM when using tensorflow in IPython notebook

## Problem

We get the following error:

    E tensorflow/stream_executor/cuda/cuda_driver.cc:965] failed to allocate 172.19M (180551680 bytes) from device: CUDA_ERROR_OUT_OF_MEMORY
    W tensorflow/core/framework/op_def_util.cc:332] Op BatchNormWithGlobalNormalization is deprecated. It will cease to work in GraphDef version 9. Use tf.nn.batch_normalization().
    E tensorflow/stream_executor/cuda/cuda_dnn.cc:348] could not create cudnn handle: CUDNN_STATUS_INTERNAL_ERROR
    E tensorflow/stream_executor/cuda/cuda_dnn.cc:315] could not destroy cudnn handle: CUDNN_STATUS_BAD_PARAM
    F tensorflow/core/kernels/conv_ops.cc:457] Check failed: stream->parent()->GetConvolveAlgorithms(&algorithms) 

## Reason

If `with` statement is used to handle a session, it seems the IPython notebook does not release the resources after it is finished. Since the first call of `tf.session()` allocates 95% of the VRAM, it fails on the second time.

## Solution

Use `tf.InteractiveSession()` and `sess.close()`
