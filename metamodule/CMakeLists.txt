cmake_minimum_required(VERSION 3.22)

if(NOT "${METAMODULE_SDK_DIR}" STREQUAL "")
	message("METAMODULE_SDK_DIR set by CMake variable ${METAMODULE_SDK_DIR}")
elseif (DEFINED ENV{METAMODULE_SDK_DIR})
    set(METAMODULE_SDK_DIR "$ENV{METAMODULE_SDK_DIR}")
	message("METAMODULE_SDK_DIR set by environment variable ${METAMODULE_SDK_DIR}")
else()
    set(METAMODULE_SDK_DIR "${CMAKE_CURRENT_LIST_DIR}/../metamodule-plugin-sdk")
	message("METAMODULE_SDK_DIR set to default: ${METAMODULE_SDK_DIR}")
endif()

include(${METAMODULE_SDK_DIR}/plugin.cmake)

project(FrozenWastelandMetaModulePlugin
    DESCRIPTION "FrequencyDomain Plugin for MetaModule" 
    LANGUAGES   C CXX ASM
)

add_library(FrequencyDomain STATIC)

set(SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/../src)


target_sources(FrequencyDomain
    PRIVATE
	${SOURCE_DIR}/FrequencyDomain.cpp
	${SOURCE_DIR}/component/menu.cpp
	${SOURCE_DIR}/controller/BallOfConfusion.cpp
	${SOURCE_DIR}/controller/BoxOfRevelation.cpp
	${SOURCE_DIR}/controller/DanceThisMeshAround.cpp
	${SOURCE_DIR}/controller/DelayedReaction.cpp
	${SOURCE_DIR}/controller/FreudianSlip.cpp
	${SOURCE_DIR}/controller/GrainsOfWrath.cpp
	${SOURCE_DIR}/controller/HarmonicConvergence.cpp
	${SOURCE_DIR}/controller/HeatOfTheMoment.cpp
	${SOURCE_DIR}/controller/Morphology.cpp
	${SOURCE_DIR}/model/fft/kiss_fft.c
	${SOURCE_DIR}/model/noise/noise.cpp
	${SOURCE_DIR}/view/BallOfConfusion.cpp
	${SOURCE_DIR}/view/BoxOfRevelation.cpp
	${SOURCE_DIR}/view/DanceThisMeshAround.cpp
	${SOURCE_DIR}/view/DelayedReaction.cpp
	${SOURCE_DIR}/view/FreudianSlip.cpp
	${SOURCE_DIR}/view/GrainsOfWrath.cpp
	${SOURCE_DIR}/view/HarmonicConvergence.cpp
	${SOURCE_DIR}/view/HeatOfTheMoment.cpp
	${SOURCE_DIR}/view/Morphology.cpp
)

target_include_directories(FrequencyDomain
	PRIVATE
	${SOURCE_DIR}
	${SOURCE_DIR}/ui
	${SOURCE_DIR}/model
	${SOURCE_DIR}/dsp-compressor
	${SOURCE_DIR}/dsp-delay
	${SOURCE_DIR}/dsp-filter/utils 
	${SOURCE_DIR}/dsp-filter/filters 
	${SOURCE_DIR}/dsp-filter/third-party/falco
)

set_property(TARGET FrequencyDomain PROPERTY CXX_STANDARD 23)

if("${INSTALL_DIR}" STREQUAL "")
    set(INSTALL_DIR ${CMAKE_CURRENT_LIST_DIR}/metamodule-plugins)
endif()

create_plugin(
    SOURCE_LIB      FrequencyDomain                                          # This is the cmake target name (defined in add_target)
    PLUGIN_NAME     FrequencyDomain                                          # This must match the brand "slug" used in VCV Rack
    PLUGIN_JSON     ${CMAKE_CURRENT_LIST_DIR}/../plugin.json                      # Path to the plugin.json file used by VCV Rack
    SOURCE_ASSETS   ${CMAKE_CURRENT_LIST_DIR}/assets               # Path to the assets/ dir containing the PNGs
    DESTINATION     ${INSTALL_DIR}                                 # Dir where plugin .mmplugin file should be put
)

