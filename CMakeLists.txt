cmake_minimum_required(VERSION 3.4 FATAL_ERROR)
project(RaftLib)
set( version 0.7a )
set( CMAKE_INCLUDE_CURRENT_DIR ON )
list( APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake )

include( CheckSTD )

option( USESCOTCH "Use Scotch partitioning library" )
find_package( Scotch )

option( USEQTHREAD "Use QThread threading library" false )
find_package( QThreads )

set(THREADS_PREFER_PTHREAD_FLAG TRUE)
find_package( Threads )

add_subdirectory( src )
add_subdirectory( raftinc )
add_subdirectory( examples )
add_subdirectory( benchmarks )

option(RAFTLIB_BuildTests "Build the RaftLib tests" OFF)
if( RAFTLIB_BuildTests )
    enable_testing()
    add_subdirectory( testsuite )
endif()

##
# install main headers in ${prefix}/include dir
##
set( MAINHEADERS 
     raft 
     raftio 
     raftmath 
     raftrandom
     raftstat )
foreach( HFILE ${MAINHEADERS} )
 install( FILES ${CMAKE_CURRENT_SOURCE_DIR}/${HFILE}  DESTINATION ${CMAKE_INSTALL_PREFIX}/include )
endforeach( HFILE ${MAINHEADERS} )

