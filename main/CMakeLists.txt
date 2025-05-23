file(GLOB_RECURSE NESTED_SRC
       **.cpp
)

#if build variant is "devel", set to "fibonacci"
if (BUILD_VARIANT STREQUAL "devel")
    set(BUILD_VARIANT "mothership")
    #idf_build_set_property(COMPILE_OPTIONS "-DHW_TYPE=1" APPEND) #mothership
    idf_build_set_property(COMPILE_OPTIONS "-DHW_TYPE=2" APPEND) #rover
endif()

list(APPEND INCLUDE_DIRS ".")
list(APPEND INCLUDE_DIRS "uart")
list(APPEND INCLUDE_DIRS "packet_processing")
list(APPEND INCLUDE_DIRS "device_manager")
list(APPEND INCLUDE_DIRS "gps")
list(APPEND INCLUDE_DIRS "modbus")
#list(APPEND INCLUDE_DIRS "${BUILD_VARIANT}")

idf_component_register(
    SRCS ${NESTED_SRC}
    INCLUDE_DIRS ${INCLUDE_DIRS}
    REQUIRES lwip esp_wifi heap json bootloader_support kd_common esp_http_client wifi_provisioning esp_driver_uart protobufs nvs_flash
)

idf_build_set_property(COMPILE_OPTIONS "-Wno-missing-field-initializers" APPEND)
idf_build_set_property(COMPILE_OPTIONS "-Wno-error=parentheses" APPEND)
idf_build_set_property(COMPILE_OPTIONS "-Wno-write-strings" APPEND)

idf_build_set_property(COMPILE_OPTIONS "-DDEVICE_NAME_PREFIX=\"CLOCK\"" APPEND)
idf_build_set_property(COMPILE_OPTIONS "-DKD_COMMON_CRYPTO_DISABLE" APPEND)