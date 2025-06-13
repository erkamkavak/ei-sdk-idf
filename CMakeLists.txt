cmake_minimum_required(VERSION 3.16)

# Set component name
set(COMPONENT_NAME "ei_sdk")

# Define all source files
set(SRCS
    # Edge Impulse SDK core files
    "edge-impulse-sdk/classifier/ei_run_classifier.h"
    "edge-impulse-sdk/dsp/numpy.hpp"
    "edge-impulse-sdk/porting/ei_classifier_porting.h"
    
    # Model files
    "model-parameters/model_metadata.h"
    "model-parameters/model_variables.h"
    "tflite-model/tflite_learn_43_compiled.cpp"
    
    # Kiss FFT sources (required for DSP)
    "edge-impulse-sdk/dsp/kissfft/kiss_fft.c"
    "edge-impulse-sdk/dsp/kissfft/tools/kiss_fftr.c"
    
    # TensorFlow Lite Micro core sources
    "edge-impulse-sdk/tensorflow/lite/micro/micro_allocator.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_context.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_graph.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_interpreter.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_profiler.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_resource_variable.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_string.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_time.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/micro_utils.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/recording_micro_allocator.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/system_setup.cc"
    
    # TensorFlow Lite Micro memory management
    "edge-impulse-sdk/tensorflow/lite/micro/memory_planner/greedy_memory_planner.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/memory_planner/linear_memory_planner.cc"
    
    # TensorFlow Lite Micro kernels (operators)
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/conv.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/fully_connected.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/pooling.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/reshape.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/softmax.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/activations.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/add.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/arg_min_max.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/batch_to_space_nd.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/cast.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/ceil.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/circular_buffer.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/comparisons.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/concatenation.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/conv_common.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/cumsum.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/depth_to_space.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/depthwise_conv.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/depthwise_conv_common.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/dequantize.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/detection_postprocess.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/elementwise.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/elu.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/ethosu.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/exp.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/expand_dims.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/fill.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/floor.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/floor_div.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/floor_mod.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/gather.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/gather_nd.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/hard_swish.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/if.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/kernel_runner.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/kernel_util.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/l2norm.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/l2_pool_2d.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/leaky_relu.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/logical.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/logistic.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/maximum_minimum.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/mul.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/neg.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/pack.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/pad.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/prelu.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/quantize.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/quantize_common.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/reduce.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/resize_bilinear.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/resize_nearest_neighbor.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/round.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/shape.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/slice.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/space_to_batch_nd.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/space_to_depth.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/split.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/split_v.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/squeeze.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/strided_slice.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/sub.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/svdf.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/svdf_common.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/tanh.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/transpose.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/transpose_conv.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/unpack.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/zeros_like.cc"
    
    # TensorFlow Lite core files
    "edge-impulse-sdk/tensorflow/lite/core/api/error_reporter.cc"
    "edge-impulse-sdk/tensorflow/lite/core/api/flatbuffer_conversions.cc"
    "edge-impulse-sdk/tensorflow/lite/core/api/op_resolver.cc"
    "edge-impulse-sdk/tensorflow/lite/core/api/tensor_utils.cc"
    "edge-impulse-sdk/tensorflow/lite/kernels/internal/quantization_util.cc"
    "edge-impulse-sdk/tensorflow/lite/kernels/kernel_util.cc"
    "edge-impulse-sdk/tensorflow/lite/schema/schema_utils.cc"
    
    # Additional TensorFlow Lite Micro files
    "edge-impulse-sdk/tensorflow/lite/c/common.c"
    "edge-impulse-sdk/tensorflow/lite/micro/arena_allocator/single_arena_buffer_allocator.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/tflite_bridge/micro_error_reporter.cc"
    "edge-impulse-sdk/tensorflow/lite/micro/tflite_bridge/flatbuffer_conversions_bridge.cc"
)

# Define include directories
set(INCLUDE_DIRS
    "."
    "edge-impulse-sdk"
    "edge-impulse-sdk/third_party/flatbuffers/include"
    "edge-impulse-sdk/third_party/gemmlowp"
    "edge-impulse-sdk/third_party/ruy"
    "edge-impulse-sdk/tensorflow/lite/micro/tools/make/downloads/flatbuffers/include"
    "model-parameters"
    "tflite-model"
)

# Define preprocessor definitions
set(COMPILE_DEFINITIONS
    "EI_CLASSIFIER_TFLITE_ENABLE_ESP_NN=1"
    "EI_CLASSIFIER_TFLITE_ENABLE_CMSIS_NN=0"
    "TF_LITE_DISABLE_X86_NEON=1"
    "TF_LITE_STATIC_MEMORY=1"
    "TF_LITE_MCU_DEBUG_LOG=1"
    "EIDSP_USE_CMSIS_DSP=0"
    "EIDSP_LOAD_CMSIS_DSP_SOURCES=0"
    "EI_PORTING_ESP32=1"
    "__STATIC_FORCEINLINE=static inline"
)

# Register the component
idf_component_register(
    SRCS ${SRCS}
    INCLUDE_DIRS ${INCLUDE_DIRS}
    REQUIRES "esp_common" "freertos" "esp_timer" "esp_system"
)

# Add compile definitions
target_compile_definitions(${COMPONENT_LIB} PRIVATE ${COMPILE_DEFINITIONS})

# Set C++ standard
target_compile_features(${COMPONENT_LIB} PRIVATE cxx_std_11)

# Add compiler flags for optimization and compatibility
target_compile_options(${COMPONENT_LIB} PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-std=c++11>
    $<$<COMPILE_LANGUAGE:CXX>:-fno-rtti>
    $<$<COMPILE_LANGUAGE:CXX>:-fno-exceptions>
    $<$<COMPILE_LANGUAGE:CXX>:-fno-threadsafe-statics>
    $<$<COMPILE_LANGUAGE:CXX>:-fmessage-length=0>
    $<$<COMPILE_LANGUAGE:CXX>:-DTF_LITE_STATIC_MEMORY>
    $<$<COMPILE_LANGUAGE:CXX>:-DTF_LITE_DISABLE_X86_NEON>
    $<$<COMPILE_LANGUAGE:C>:-std=c99>
) 
