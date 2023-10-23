cmake_minimum_required(VERSION 3.22)

project(VulkanGraphics)

# add dependencies
include(cmake/CPM.cmake)

# set cpm cache to accelerate regenerate project files
set(CPM_SOURCE_CACHE
    "${CMAKE_CURRENT_SOURCE_DIR}/_cpmcache"
    CACHE PATH "Directory to download CPM dependencies"
	FORCE
)

set(CMAKE_SKIP_INSTALL_RULES YES)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "$<0:>${CMAKE_CURRENT_SOURCE_DIR}/Binaries/")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "$<0:>${CMAKE_CURRENT_SOURCE_DIR}/Binaries/")

set(CMAKE_DEBUG_POSTFIX "_d")

# Find Vulkan SDK
find_package(Vulkan REQUIRED)

CPMAddPackage(
        NAME assimp
        GITHUB_REPOSITORY assimp/assimp
        VERSION 5.3.1
        OPTIONS 
        "ASSIMP_BUILD_TESTS OFF" 
        "ASSIMP_INSTALL OFF" 
        "ASSIMP_INSTALL_PDB OFF"
        "USE_STATIC_CRT OFF"
        )  

#get SDL
CPMAddPackage(
    NAME SDL
    GITHUB_REPOSITORY libsdl-org/SDL
    GIT_TAG release-2.28.4    
	OPTIONS 
	"SDL2_DISABLE_INSTALL ON" 
	"SDL2_DISABLE_UNINSTALL ON" 
	"SDL_TEST_ENABLED_BY_DEFAULT OFF" 
	"SDL_STATIC OFF"
	)
 

set(ADDITIONAL_INCLUDES "Source")

add_executable (test Source/Trials/test.cpp Source/common/CustomAssetsPath.h)
target_link_libraries(
    test PRIVATE
    SDL2::SDL2
    SDL2::SDL2main
    assimp::assimp
)
#add_dependencies(test assimp::assimp)

target_include_directories(test PRIVATE ${ADDITIONAL_INCLUDES})
set_property(TARGET test PROPERTY CXX_STANDARD 17)

add_library(RaptorFoundation STATIC
    Source/raptor/foundation/array.hpp
    Source/raptor/foundation/assert.cpp
    Source/raptor/foundation/assert.hpp
    Source/raptor/foundation/bit.cpp
    Source/raptor/foundation/bit.hpp
    Source/raptor/foundation/blob_serialization.cpp
    Source/raptor/foundation/blob_serialization.hpp
    Source/raptor/foundation/blob.hpp
    Source/raptor/foundation/camera.cpp
    Source/raptor/foundation/camera.hpp
    Source/raptor/foundation/color.cpp
    Source/raptor/foundation/color.hpp
    Source/raptor/foundation/data_structures.cpp
    Source/raptor/foundation/data_structures.hpp
    Source/raptor/foundation/file.cpp
    Source/raptor/foundation/file.hpp
    Source/raptor/foundation/gltf.cpp
    Source/raptor/foundation/gltf.hpp
    Source/raptor/foundation/hash_map.hpp
    Source/raptor/foundation/log.cpp
    Source/raptor/foundation/log.hpp
    Source/raptor/foundation/memory_utils.hpp
    Source/raptor/foundation/memory.cpp
    Source/raptor/foundation/memory.hpp
    Source/raptor/foundation/numerics.cpp
    Source/raptor/foundation/numerics.hpp
    Source/raptor/foundation/platform.hpp
    Source/raptor/foundation/process.cpp
    Source/raptor/foundation/process.hpp
    Source/raptor/foundation/relative_data_structures.hpp
    Source/raptor/foundation/resource_manager.cpp
    Source/raptor/foundation/resource_manager.hpp
    Source/raptor/foundation/serialization.cpp
    Source/raptor/foundation/serialization.hpp
    Source/raptor/foundation/service_manager.cpp
    Source/raptor/foundation/service_manager.hpp
    Source/raptor/foundation/service.cpp
    Source/raptor/foundation/service.hpp
    Source/raptor/foundation/string.cpp
    Source/raptor/foundation/string.hpp
    Source/raptor/foundation/time.cpp
    Source/raptor/foundation/time.hpp
)

set_property(TARGET RaptorFoundation PROPERTY CXX_STANDARD 17)

target_compile_definitions(RaptorFoundation PRIVATE
    _CRT_SECURE_NO_WARNINGS

    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

target_link_libraries(RaptorFoundation PRIVATE
    SDL2::SDL2
    assimp::assimp
)

target_include_directories(RaptorFoundation PUBLIC
    Source
    Source/raptor
)

add_library(RaptorApp STATIC
    Source/raptor/application/application.cpp
    Source/raptor/application/application.hpp
    Source/raptor/application/game_camera.cpp
    Source/raptor/application/game_camera.hpp
    Source/raptor/application/game_application.cpp
    Source/raptor/application/game_application.hpp
    Source/raptor/application/input.cpp
    Source/raptor/application/input.hpp
    Source/raptor/application/keys.cpp
    Source/raptor/application/keys.hpp
    Source/raptor/application/window.cpp
    Source/raptor/application/window.hpp
)

set_property(TARGET RaptorApp PROPERTY CXX_STANDARD 17)

target_compile_definitions(RaptorApp PRIVATE
    _CRT_SECURE_NO_WARNINGS

    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

target_link_libraries(RaptorApp PRIVATE
    SDL2::SDL2
    assimp::assimp
)

target_include_directories(RaptorApp PRIVATE
    Source
    Source/raptor
    )

add_library(RaptorExternal STATIC
    Source/external/tlsf.c

    Source/external/json.hpp

    Source/external/imgui/imconfig.h
    Source/external/imgui/imgui_draw.cpp
    Source/external/imgui/imgui_impl_sdl.cpp
    Source/external/imgui/imgui_impl_sdl.h
    Source/external/imgui/imgui_internal.h
    Source/external/imgui/imgui_memory_editor.h
    Source/external/imgui/imgui_tables.cpp
    Source/external/imgui/imgui_widgets.cpp
    Source/external/imgui/imgui.cpp
    Source/external/imgui/imgui.h
    Source/external/imgui/imstb_rectpack.h
    Source/external/imgui/imstb_textedit.h
    Source/external/imgui/imstb_truetype.h
    Source/external/imgui/stb_image.h
    Source/external/imgui/TextEditor.cpp
    Source/external/imgui/TextEditor.h

    Source/external/tracy/tracy/Tracy.hpp
    Source/external/tracy/tracy/TracyVulkan.hpp
    Source/external/tracy/TracyClient.cpp

    Source/external/enkiTS/LockLessMultiReadPipe.h
    Source/external/enkiTS/TaskScheduler.cpp
    Source/external/enkiTS/TaskScheduler.h

    Source/external/meshoptimizer/allocator.cpp
    Source/external/meshoptimizer/clusterizer.cpp
    Source/external/meshoptimizer/indexcodec.cpp
    Source/external/meshoptimizer/indexgenerator.cpp
    Source/external/meshoptimizer/meshoptimizer.h
    Source/external/meshoptimizer/overdrawanalyzer.cpp
    Source/external/meshoptimizer/overdrawoptimizer.cpp
    Source/external/meshoptimizer/simplifier.cpp
    Source/external/meshoptimizer/spatialorder.cpp
    Source/external/meshoptimizer/stripifier.cpp
    Source/external/meshoptimizer/vcacheanalyzer.cpp
    Source/external/meshoptimizer/vcacheoptimizer.cpp
    Source/external/meshoptimizer/vertexcodec.cpp
    Source/external/meshoptimizer/vertexfilter.cpp
    Source/external/meshoptimizer/vfetchanalyzer.cpp
    Source/external/meshoptimizer/vfetchoptimizer.cpp
)

target_link_libraries(RaptorExternal PRIVATE
    SDL2::SDL2
    assimp::assimp
)

if (WIN32)
    target_sources(RaptorExternal PRIVATE
        Source/external/StackWalker.cpp)
endif()

target_compile_definitions(RaptorExternal PRIVATE
    TRACY_ENABLE
    TRACY_ON_DEMAND
    TRACY_NO_SYSTEM_TRACING
)

target_include_directories(RaptorExternal PRIVATE
    Source/raptor
    Source/external
)

set_property(TARGET RaptorExternal PROPERTY CXX_STANDARD 17)

function (add_sample SAMPLE_NAME)
    cmake_parse_arguments (SAMPLE "" "ROOT_PATH;DATA_PATH" "" ${ARGN})

    file(GLOB_RECURSE SAMPLE_SOURCES "${SAMPLE_ROOT_PATH}/sources/*.cpp")
    file(GLOB_RECURSE SAMPLE_HEADERS 
    "${SAMPLE_ROOT_PATH}/sources/*.hpp"
    "${SAMPLE_ROOT_PATH}/sources/*.h"
    )

    add_executable(${SAMPLE_NAME} ${SAMPLE_SOURCES} ${SAMPLE_HEADERS})
    set_property(TARGET ${SAMPLE_NAME} PROPERTY CXX_STANDARD 17)
    if (WIN32)
        target_compile_definitions(${SAMPLE_NAME} PRIVATE
            _CRT_SECURE_NO_WARNINGS
            WIN32_LEAN_AND_MEAN
            NOMINMAX)
    endif()
    target_compile_definitions(${SAMPLE_NAME} PRIVATE
        RAPTOR_WORKING_FOLDER="${SAMPLE_ROOT_PATH}"
        RAPTOR_SHADER_FOLDER="${SAMPLE_ROOT_PATH}/shaders/"
        RAPTOR_DATA_FOLDER="${SAMPLE_DATA_PATH}"
        ) 
    target_include_directories(${SAMPLE_NAME} PRIVATE
        ${SAMPLE_ROOT_PATH}/sources
    )
    if (NOT WIN32)
        target_link_libraries(${SAMPLE_NAME} PRIVATE
            pthread
        )
    endif()
    target_link_libraries(${SAMPLE_NAME} PRIVATE
        SDL2::SDL2
        SDL2::SDL2main
        assimp::assimp
        Vulkan::Vulkan
    )
    target_link_libraries(${SAMPLE_NAME} PRIVATE
        RaptorFoundation
        RaptorExternal
        RaptorApp
    )
    

endfunction()

add_sample(RaptorPlayground ROOT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/Source/RaptorPlayground" DATA_PATH "${CMAKE_CURRENT_SOURCE_DIR}/Assets/data")
