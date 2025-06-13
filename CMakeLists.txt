set(component_srcs 
    "edge-impulse-sdk/porting/espressif/ei_classifier_porting.cpp"
    "edge-impulse-sdk/porting/espressif/debug_log.cpp"
)

# Add Edge Impulse SDK source files (selective compilation)
file(GLOB_RECURSE EI_CLASSIFIER_SOURCES 
    "edge-impulse-sdk/classifier/*.cpp"
)

file(GLOB_RECURSE EI_DSP_SOURCES 
    "edge-impulse-sdk/dsp/*.cpp" 
)

# Add CMSIS DSP sources
file(GLOB_RECURSE CMSIS_DSP_SOURCES
    "edge-impulse-sdk/CMSIS/DSP/Source/BasicMathFunctions/*.c"
    "edge-impulse-sdk/CMSIS/DSP/Source/FastMathFunctions/*.c"
    "edge-impulse-sdk/CMSIS/DSP/Source/StatisticsFunctions/*.c"
    "edge-impulse-sdk/CMSIS/DSP/Source/TransformFunctions/*.c"
    "edge-impulse-sdk/CMSIS/DSP/Source/CommonTables/*.c"
    "edge-impulse-sdk/CMSIS/DSP/Source/SupportFunctions/*.c"
)

# Add TensorFlow Lite Micro sources
file(GLOB_RECURSE TFLITE_SOURCES
    "edge-impulse-sdk/tensorflow/lite/micro/*.cpp"
    "edge-impulse-sdk/tensorflow/lite/micro/kernels/*.cpp" 
    "edge-impulse-sdk/tensorflow/lite/micro/memory_planner/*.cpp"
    "edge-impulse-sdk/tensorflow/lite/c/*.c"
    "edge-impulse-sdk/tensorflow/lite/core/api/*.cpp"
)

# ESP-NN optimized operations
file(GLOB_RECURSE ESP_NN_SOURCES
    "edge-impulse-sdk/porting/espressif/ESP-NN/src/*.cpp"
)

# Combine all sources
list(APPEND component_srcs ${EI_CLASSIFIER_SOURCES})
list(APPEND component_srcs ${EI_DSP_SOURCES})
list(APPEND component_srcs ${CMSIS_DSP_SOURCES})
list(APPEND component_srcs ${TFLITE_SOURCES})
list(APPEND component_srcs ${ESP_NN_SOURCES})

set(component_includes 
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
)

idf_component_register(
    SRCS ${component_srcs}
    INCLUDE_DIRS ${component_includes}
    REQUIRES "esp-nn" "driver" "esp_timer" "freertos" "log" "heap"
    PRIV_REQUIRES "spi_flash"
)

# Compiler definitions
target_compile_definitions(${COMPONENT_LIB} PRIVATE
    -DEI_PORTING_ESPRESSIF=1
    -DEIDSP_USE_CMSIS_DSP=1
    -DEI_CLASSIFIER_TFLITE_ENABLE_ESP_NN=1
    -DTF_LITE_STATIC_MEMORY=1
    -DTF_LITE_STRIP_ERROR_STRINGS=1
    -DNDEBUG=1
    -DEI_CLASSIFIER_TFLITE_ENABLE_ESP32=1
)

# Optimization flags
target_compile_options(${COMPONENT_LIB} PRIVATE
    -Os
    -ffunction-sections
    -fdata-sections
    -fno-exceptions
    -fno-rtti
    -Wno-error
    -Wno-unused-function
    -Wno-unused-variable
    -Wno-unused-but-set-variable
    -Wno-missing-field-initializers
)
