cmake_minimum_required(VERSION 3.22)

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

#find_package (glfw3 REQUIRED)
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

CPMAddPackage(
        NAME assimp
        GITHUB_REPOSITORY assimp/assimp
        VERSION 5.3.1
        OPTIONS 
        "ASSIMP_BUILD_TESTS OFF" 
        "ASSIMP_INSTALL OFF" 
        "ASSIMP_INSTALL_PDB OFF"
        "USE_STATIC_CRT ON"
        )    

project(VulkanGraphics)

function(add_sample SAMPLE_NAME)
#cmake_parse_arguments (SAMPLE "" "SHADER" "LIBS;TEXTURES;MODELS" ${ARGN})

endfunction()

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