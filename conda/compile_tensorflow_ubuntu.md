# Compile Tensorflow in Ubuntu

    bazel build -c opt --config=cuda //tensorflow/cc:tutorials_example_trainer --verbose_failures
    
fails with

    ERROR: /home/peter/Downloads/sources/tensorflow/tensorflow/core/kernels/BUILD:1664:1: output 'tensorflow/core/kernels/_objs/training_ops_gpu/tensorflow/core/kernels/training_ops_gpu.cu.o' was not created.
    ERROR: /home/peter/Downloads/sources/tensorflow/tensorflow/core/kernels/BUILD:1664:1: not all outputs were created.
    
Solution is on github: https://github.com/tensorflow/tensorflow/issues/1066

Apparently, one has to change the compiler flags in 


    diff --git a/third_party/gpus/crosstool/CROSSTOOL b/third_party/gpus/crosstool/CROSSTOOL
    index dfde7cd..15fa9fd 100644
    --- a/third_party/gpus/crosstool/CROSSTOOL
    +++ b/third_party/gpus/crosstool/CROSSTOOL
    @@ -46,6 +46,9 @@ toolchain {
       # Use "-std=c++11" for nvcc. For consistency, force both the host compiler
       # and the device compiler to use "-std=c++11".
       cxx_flag: "-std=c++11"
    +  cxx_flag: "-D_MWAITXINTRIN_H_INCLUDED"
    +  cxx_flag: "-D_FORCE_INLINES"
    +  cxx_flag: "-D__STRICT_ANSI__"
       linker_flag: "-lstdc++"
       linker_flag: "-B/usr/bin/"
