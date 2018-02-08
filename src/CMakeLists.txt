
set(CPP_SRC_FILES 
    affinity.cpp
    allocate.cpp
    basicparallel.cpp
    common.cpp
    demangle.cpp
    dynalloc.cpp
    fifo.cpp
    graphtools.cpp
    kernelcontainer.cpp
    kernel.cpp
    kernelexception.cpp
    kernel_pair_t.cpp
    kernel_wrapper.cpp
    kpair.cpp
    leastusedfirst.cpp
    mapbase.cpp
    map.cpp
    mapexception.cpp
    noparallel.cpp
    parallelk.cpp
    partition_basic.cpp
    partition_dummy.cpp
    partition_scotch.cpp
    pointer.cpp
    poolschedule.cpp
    port.cpp
    portexception.cpp
    port_info.cpp
    portiterator.cpp
    porttemplate.cpp
    roundrobin.cpp
    schedule.cpp
    signal.cpp
    signaldata.cpp
    simpleschedule.cpp
    stdalloc.cpp
    submap.cpp
    systemsignalhandler.cpp
)

set( HELPER_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../helpers )

##
# helper exec to get the L1 cache size from Linux/OS X/Win
##
execute_process( COMMAND ${CMAKE_CXX_COMPILER} ${HELPER_DIR}/cacheinfo.cpp -o ${HELPER_DIR}/cacheinfo)
execute_process( COMMAND ${HELPER_DIR}/cacheinfo 
                 COMMAND tr -d '\n' 
                 OUTPUT_VARIABLE L1D_LINE_SIZE )
execute_process( COMMAND rm -f ${HELPER_DIR}/cacheinfo )                 

if( NOT L1D_LINE_SIZE )
    set( L1D_LINE_SIZE 64 )
    message( INFO " Couldn't detect cache line size, set to: ${L1D_LINE_SIZE}" )
else( NOT L1D_LINE_SIZE )
    message( INFO " Detected cache line size, set to: ${L1D_LINE_SIZE}" )
endif( NOT L1D_LINE_SIZE )

##
# for cache line size
##
add_library( raft ${CPP_SRC_FILES} )
target_compile_definitions(raft 
    PRIVATE -DLIBCOMPILE=1 -DRDTSCP=1 
    PUBLIC "-DL1D_CACHE_LINE_SIZE=${L1D_LINE_SIZE}") 
target_include_directories(raft 
    PUBLIC 
    ${PROJECT_SOURCE_DIR}/raftinc 
    ${PROJECT_SOURCE_DIR}
)

target_link_libraries(raft PUBLIC 
                       ${CMAKE_SCOTCH_LIBS}
                       ${CMAKE_QTHREAD_LIBS} 
                       ${CMAKE_THREAD_LIBS_INIT} 
                       ${CMAKE_RT_LIBS}
                     )


# Enable warnings if using clang or gcc.
if ( "${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang" 
  OR "${CMAKE_CXX_COMPILER_ID}" MATCHES "GNU" )

target_compile_options( raft
                        PUBLIC
                        "-W" "-Wall" "-Wextra" "-Wpedantic" )

endif()

install( TARGETS raft
         ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib )
