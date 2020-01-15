# Target name
set (target Sources)

find_package(OpenGL REQUIRED)

# Includes
include_directories(${CMAKE_SOURCE_DIR}/Includes)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/glfw/include)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/glad/include)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/imgui)
include_directories(${CMAKE_SOURCE_DIR}/Libraries/SFML/include)
include_directories(${OPENGL_INCLUDE_DIR})

link_libraries(${OPENGL_LIBRARIES})
link_directories(${CMAKE_SOURCE_DIR}/Libraries/glfw/lib)

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

# Link libraries
if (CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    target_link_libraries(${target}
        PRIVATE
        ${DEFAULT_LINKER_OPTIONS}
        ${SFML_LIBRARY_DEPENDENCIES}
        ${SFML_OPENAL_DEPENDENCIES}
        imgui)
else()
    target_link_libraries(${target}
        PRIVATE
        ${DEFAULT_LINKER_OPTIONS}
        ${SFML_LIBRARY_DEPENDENCIES}
        ${SFML_OPENGL_DEPENDENCIES}
        ${SFML_OPENAL_DEPENDENCIES}
        ${OPENGL_LIBRARIES}
        imgui)
endif()