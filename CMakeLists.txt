cmake_minimum_required(VERSION 3.16)
idf_component_register(
    INCLUDE_DIRS edge-impulse-sdk 
    SRCS
        # SDK port layer for ESP32 – already present in the ZIP
        edge-impulse-sdk/porting/esp/ei_classifier_porting.cpp
        # Everything in model-parameters becomes its own component (see step 3)
)

# Recommended optimisation flags
target_compile_options(${COMPONENT_LIB} PRIVATE
        -O3
        -DEI_CLASSIFIER_TFLITE_ENABLE_ESP_NN=1      # use Espressif DSP kernels
        -DEI_DSP_IMAGE_MAX_AXES=3                   # shrink DSP footprint
)

