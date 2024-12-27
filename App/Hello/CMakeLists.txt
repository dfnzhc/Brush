set(ProjectName Hello)

file(GLOB_RECURSE SOURCE_FILES ./*.hpp ./*.cpp)
AssignSourceGroup(${SOURCE_FILES})

add_executable(${ProjectName} ${SOURCE_FILES})

target_link_libraries(${ProjectName}
        #    PRIVATE
        PUBLIC
        Brush
)