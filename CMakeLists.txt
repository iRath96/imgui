find_package(glfw3 3.2 REQUIRED)
find_package(OpenGL REQUIRED)

set(IMGUI_INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR})
file(GLOB IMGUI_SOURCES ${CMAKE_CURRENT_LIST_DIR}/*.cpp ${CMAKE_CURRENT_LIST_DIR}/GL/gl3w.c)
file(GLOB IMGUI_HEADERS ${CMAKE_CURRENT_LIST_DIR}/*.h)

add_library(imgui STATIC ${IMGUI_SOURCES} ${IMGUI_SOURCES})

add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GL3W)

target_include_directories(imgui
    PUBLIC ${IMGUI_INCLUDE_DIR}
    PUBLIC ${OPENGL_INCLUDE_DIR}
)

target_link_libraries(imgui glfw ${OPENGL_LIBRARIES})

if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    target_link_libraries(imgui
        "-framework CoreFoundation"
    )
else()
    target_link_libraries(imgui -ldl -lpthread)
endif()

set_target_properties(imgui PROPERTIES LINKER_LANGUAGE CXX)
set_target_properties(imgui PROPERTIES FOLDER 3rdparty)

set(IMGUI_LIBRARIES imgui)
