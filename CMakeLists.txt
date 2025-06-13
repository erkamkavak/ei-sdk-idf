cmake_minimum_required(VERSION 3.5)

set(ei_sdk_dir "${CMAKE_CURRENT_SOURCE_DIR}/edge-impulse-sdk")

# Core Edge Impulse SDK sources
file(GLOB ei_classifier_srcs
    "${ei_sdk_dir}/classifier/*.cpp"
    "${ei_sdk_dir}/classifier/*.c"
)

file(GLOB ei_dsp_srcs
    "${ei_sdk_dir}/dsp/*.cpp"
    "${ei_sdk_dir}/dsp/*.c"
)

# Porting layer for Espressif
file(GLOB ei_porting_srcs
    "${ei_sdk_dir}/porting/espressif/*.cpp"
    "${ei_sdk_dir}/porting/espressif/*.c"
)

# CMSIS DSP sources (selective)
file(GLOB cmsis_dsp_srcs
    "${ei_sdk_dir}/CMSIS/DSP/Source/BasicMathFunctions/*.c"
    "${ei_sdk_dir}/CMSIS/DSP/Source/FastMathFunctions/*.c"
    "${ei_sdk_dir}/CMSIS/DSP/Source/StatisticsFunctions/*.c"
    "${ei_sdk_dir}/CMSIS/DSP/Source/TransformFunctions/*.c"
    "${ei_sdk_dir}/CMSIS/DSP/Source/CommonTables/*.c"
    "${ei_sdk_dir}/CMSIS/DSP/Source/SupportFunctions/*.c"
)

# TensorFlow Lite Micro sources
file(GLOB tflite_micro_srcs
    "${ei_sdk_dir}/tensorflow/lite/micro/*.cpp"
    "${ei_sdk_dir}/tensorflow/lite/micro/*.c"
)

file(GLOB tflite_kernels_srcs
    "${ei_sdk_dir}/tensorflow/lite/micro/kernels/*.cpp"
    "${ei_sdk_dir}/tensorflow/lite/micro/kernels/*.c"
)

file(GLOB tflite_core_srcs
    "${ei_sdk_dir}/tensorflow/lite/c/*.c"
    "${ei_sdk_dir}/tensorflow/lite/core/api/*.cpp"
)

# ESP-NN optimized kernels
file(GLOB esp_nn_srcs
    "${ei_sdk_dir}/porting/espressif/ESP-NN/src/*.cpp"
    "${ei_sdk_dir}/porting/espressif/ESP-NN/src/*.c"
)

set(lib_srcs
    ${ei_classifier_srcs}
    ${ei_dsp_srcs}
    ${ei_porting_srcs}
    ${cmsis_dsp_srcs}
    ${tflite_micro_srcs}
    ${tflite_kernels_srcs}
    ${tflite_core_srcs}
    ${esp_nn_srcs}
)

set(priv_req)

# Include component requirements which were introduced after IDF version 4.1
if("${IDF_VERSION_MAJOR}.${IDF_VERSION_MINOR}" VERSION_GREATER "4.1")
    list(APPEND priv_req esp_timer driver)
endif()

idf_component_register(
    SRCS "${lib_srcs}"
    INCLUDE_DIRS "."
                 "edge-impulse-sdk"
                 "edge-impulse-sdk/classifier"
                 "edge-impulse-sdk/dsp"
                 "edge-impulse-sdk/porting"
                 "edge-impulse-sdk/porting/espressif"
                 "edge-impulse-sdk/tensorflow"
                 "edge-impulse-sdk/tensorflow/lite"
                 "edge-impulse-sdk/tensorflow/lite/micro"
                 "edge-impulse-sdk/CMSIS/Core/Include"
                 "edge-impulse-sdk/CMSIS/DSP/Include"
                 "edge-impulse-sdk/third_party/flatbuffers/include"
                 "edge-impulse-sdk/third_party/gemmlowp"
                 "edge-impulse-sdk/third_party/ruy"
    REQUIRES esp-nn
    PRIV_REQUIRES ${priv_req}
)

# Reduce the level of paranoia to be able to compile Edge Impulse sources
target_compile_options(${COMPONENT_LIB} PRIVATE
    -Wno-error=attributes
    -Wno-error=shadow
    -Wno-maybe-uninitialized
    -Wno-missing-field-initializers
    -Wno-error=sign-compare
    -Wno-error=double-promotion
    -Wno-type-limits
    -Wno-unused-function
    -Wno-unused-variable
    -Wno-unused-but-set-variable
)

if("${IDF_VERSION_MAJOR}.${IDF_VERSION_MINOR}" VERSION_GREATER_EQUAL "5.0")
    target_compile_options(${COMPONENT_LIB} PRIVATE -Wno-error=stringop-overread)
endif()

# Enable ESP-NN optimizations by Espressif
target_compile_options(${COMPONENT_LIB} PRIVATE -DESP_NN)

set(common_flags 
    -DEI_PORTING_ESPRESSIF=1
    -DEIDSP_USE_CMSIS_DSP=1
    -DEI_CLASSIFIER_TFLITE_ENABLE_ESP_NN=1
    -DTF_LITE_STATIC_MEMORY=1
    -DTF_LITE_STRIP_ERROR_STRINGS=1
    -DTF_LITE_DISABLE_X86_NEON
    -DNDEBUG=1
    -O3
    -ffunction-sections
    -fdata-sections
    -fno-unwind-tables
    -fmessage-length=0
)

target_compile_options(${COMPONENT_LIB} PRIVATE ${common_flags})
target_compile_options(${COMPONENT_LIB} PRIVATE $<$<COMPILE_LANGUAGE:CXX>:
    -std=c++11 -fno-rtti -fno-exceptions -fno-threadsafe-statics -std=gnu++14>)

target_compile_options(${COMPONENT_LIB} INTERFACE -DTF_LITE_STATIC_MEMORY)
target_link_libraries(${COMPONENT_LIB} PRIVATE -lm)
