if (TARGET cmdargs) 
add_executable( pisim "pisim.cpp" )
target_link_libraries( pisim raft cmdargs)
endif()

