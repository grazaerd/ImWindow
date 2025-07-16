cmake_minimum_required(VERSION 3.5)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_C_STANDARD 17)

project(guitest)
set(CMAKE_C_FLAGS "-O3 -fuse-ld=lld -falign-functions=16 -falign-labels=16 -falign-loops=16 -falign-jumps=16 -fno-common -fno-record-gcc-switches -DNDEBUG -static -fomit-frame-pointer -fno-asynchronous-unwind-tables -fno-unwind-tables -fdata-sections -ffunction-sections -Wl,--gc-sections -Wl,-s")
set(CMAKE_CXX_FLAGS "-O3 -fuse-ld=lld -falign-functions=16 -falign-labels=16 -falign-loops=16 -falign-jumps=16 -lstdc++ -fno-common -fno-record-gcc-switches -DNDEBUG -static -fomit-frame-pointer -fno-unwind-tables -fdata-sections -ffunction-sections -Wl,--gc-sections -Wl,-s")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)


add_executable(guitest src/main.cpp)

set_target_properties(guitest PROPERTIES LINK_FLAGS "-mwindows")

set(directx "${CMAKE_CURRENT_SOURCE_DIR}/lib/DirectX")
set(easywindow "${CMAKE_CURRENT_SOURCE_DIR}/lib/EasyWindow")
set(imgui "${CMAKE_CURRENT_SOURCE_DIR}/lib/imgui")
set(imwindow "${CMAKE_CURRENT_SOURCE_DIR}/lib/ImWindow")
set(utils "${CMAKE_CURRENT_SOURCE_DIR}/lib/UtilsCollection")

add_subdirectory(${directx})
add_subdirectory(${easywindow})
add_subdirectory(${imgui})
add_subdirectory(${imwindow})
add_subdirectory(${utils})

target_compile_options(guitest PRIVATE -Wall -Wuse-after-free=3 -Wextra -Wshadow -Wnon-virtual-dtor -Wpedantic -Wold-style-cast -Wcast-align -Wunused -Woverloaded-virtual -Wpedantic -Wconversion -Wsign-conversion -Wmisleading-indentation -Wduplicated-cond -Wduplicated-branches -Wlogical-op -Wnull-dereference -Wuseless-cast -Wdouble-promotion -Wformat=2 -Wimplicit-fallthrough)

# remove warnings for third party libraries
target_compile_options(directx PRIVATE -w)
target_compile_options(easywindow PRIVATE -w)
target_compile_options(imgui PRIVATE -w)
target_compile_options(imwindow PRIVATE -w)
target_compile_options(utils PRIVATE -w)

target_include_directories(guitest SYSTEM PRIVATE ${directx})
target_include_directories(guitest SYSTEM PRIVATE ${easywindow})
target_include_directories(guitest SYSTEM PRIVATE ${imgui})
target_include_directories(guitest SYSTEM PRIVATE ${imwindow})
target_include_directories(guitest SYSTEM PRIVATE ${utils})

target_link_libraries(guitest PRIVATE directx)
target_link_libraries(guitest PRIVATE easywindow)
target_link_libraries(guitest PRIVATE imgui)
target_link_libraries(guitest PRIVATE imwindow)
target_link_libraries(guitest PRIVATE utils)
