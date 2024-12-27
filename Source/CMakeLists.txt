set(ProjectName Brush)

file(GLOB_RECURSE SOURCE_FILES ./*.hpp ./*.cpp)
AssignSourceGroup(${SOURCE_FILES})

add_library(${ProjectName} SHARED ${SOURCE_FILES})

SetCompilerFlags(${ProjectName})
SetDefaultCompileDefinitions(${ProjectName})

target_compile_features(${ProjectName} PUBLIC cxx_std_23)

add_custom_command(TARGET ${ProjectName} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "$<TARGET_FILE_DIR:${ProjectName}>/${ProjectName}.dll"
        ${${PROJECT_NAME_UPPERCASE}_BINARY_DIR})

find_package(fmt CONFIG REQUIRED)
find_package(libassert CONFIG REQUIRED)
find_package(magic_enum CONFIG REQUIRED)
find_path(STRINGZILLA_INCLUDE_DIRS "stringzilla/drafts.h")
find_package(raylib CONFIG REQUIRED)

target_link_libraries(${ProjectName}
        #    PRIVATE
        PUBLIC
        raylib
        fmt::fmt
        libassert::assert
        magic_enum::magic_enum
)

target_compile_options(${ProjectName}
        PUBLIC
        # MSVC
        $<$<CXX_COMPILER_ID:MSVC>:/DEBUG>
)

target_include_directories(${ProjectName} 
        PUBLIC 
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${STRINGZILLA_INCLUDE_DIRS}
)
