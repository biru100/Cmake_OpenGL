# Target name
set(target gui)

# Find packages
find_package(OpenGL REQUIRED)
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU" OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    find_package(glfw3 REQUIRED)
    find_package(OpenAL REQUIRED)
endif()

# Includes
include_directories(${CMAKE_SOURCE_DIR}/Includes)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/glfw/include)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/imgui)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/SFML/include)
include_directories(${OPENGL_INCLUDE_DIR})

link_libraries(${OPENGL_LIBRARIES})
link_directories(${CMAKE_SOURCE_DIR}/Libraries/glfw/lib)
if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    link_libraries(glfw3)
else()
    link_libraries(glfw)
endif()
if (CMAKE_SYSTEM_NAME MATCHES "Windows")
    set(SFML_LIBRARIES_PATH ${CMAKE_SOURCE_DIR}/Libraries/SFML/lib/Windows)
    set(SFML_OPENAL_DEPENDENCIES openal32)
elseif (CMAKE_SYSTEM_NAME MATCHES "Linux")
    set(SFML_LIBRARIES_PATH ${CMAKE_SOURCE_DIR}/Libraries/SFML/lib/Linux)
    set(SFML_OPENGL_DEPENDENCIES GL)
    set(SFML_OPENAL_DEPENDENCIES openal)
elseif (CMAKE_SYSTEM_NAME MATCHES "Darwin")
    set(SFML_LIBRARIES_PATH ${CMAKE_SOURCE_DIR}/Libraries/SFML/lib/MacOS)
    set(SFML_OPENGL_DEPENDENCIES "-framework OpenGL")
    set(SFML_OPENAL_DEPENDENCIES ${OPENAL_LIBRARY})
endif()
link_directories(${SFML_LIBRARIES_PATH})
set(SFML_LIBRARY_DEPENDENCIES sfml-audio sfml-graphics sfml-network sfml-system sfml-window)

# Includes
include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/../Libraries
)

# Sources
file(GLOB_RECURSE headers
    ${CMAKE_SOURCE_DIR}/Includes/*.h)
    
file(GLOB_RECURSE sources
    ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

# Build executable
add_executable(${target}
    ${sources})


# Compile options
target_compile_options(${target}
    PRIVATE

    PUBLIC
    ${DEFAULT_COMPILE_OPTIONS}

    INTERFACE
)
target_compile_definitions(${target}
    PRIVATE
    IMAGES_DIR="${CMAKE_SOURCE_DIR}/Assets/Images/"
)
# Link libraries
if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    target_link_libraries(${target}
        PRIVATE
        ${DEFAULT_LINKER_OPTIONS}
        ${SFML_LIBRARY_DEPENDENCIES}
        ${SFML_OPENAL_DEPENDENCIES}
        glad
        imgui)
else()
    target_link_libraries(${target}
        PRIVATE
        ${DEFAULT_LINKER_OPTIONS}
        ${SFML_LIBRARY_DEPENDENCIES}
        ${SFML_OPENGL_DEPENDENCIES}
        ${SFML_OPENAL_DEPENDENCIES}
        ${OPENGL_LIBRARIES}
        glad
        imgui)
endif()
