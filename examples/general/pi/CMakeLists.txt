if (TARGET cmdargs) 
add_executable( pisim "pisim.cpp" )
target_link_libraries( pisim
                       raft
                       cmdargs
                       ${CMAKE_SCOTCH_LIBS}
                       ${CMAKE_THREAD_LIBS_INIT} 
                       ${CMAKE_RT_LIBS} )
endif()

