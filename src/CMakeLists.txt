# Set output directory
SET(EXECUTABLE_OUTPUT_PATH ${CMAKE_INSTALL_PREFIX}/bin)
SET(LIBRARY_OUTPUT_PATH ${CMAKE_INSTALL_PREFIX}/lib)

ADD_DEFINITIONS("-D_LARGE_FILES")
ADD_DEFINITIONS("-D_FILE_OFFSET_BITS=64")
ADD_DEFINITIONS("-DMYCODESIZE=3")
ADD_DEFINITIONS("-DDEBUG")
ADD_DEFINITIONS("-DTRACE_LEVEL=1")

if (CXX0)
  MESSAGE( STATUS "HAVE_CXX0=true; hence, variable HAVE_CXX0 is set" )
  SET(STD_FLAG "-std=c++0x")
  ADD_DEFINITIONS("-DHAVE_CXX0")
else()
  MESSAGE( STATUS "HAVE_CXX0=false; hence, variable HAVE_CXX0 is unset" )
  SET(STD_FLAG "")
  ADD_DEFINITIONS("-UHAVE_CXX0")
endif() 

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g ${STD_FLAG} -isystem/usr/include -W -Wall -ffor-scope")
SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}")

INCLUDE_DIRECTORIES("${PROJECT_SOURCE_DIR}/src")

SET( LIB_CSWA_SRC
        cmd.h cmd.c
        util.h util.cpp 
        timer.h timer.cpp 
        thpool.h thpool.c
        mempool.h mempool.cpp 
        mfstream.h mfstream.cpp 
        gzfilebuf.h index.h
        htable.h htable.cpp 
        n_gram.h n_gram.cpp
        dictionary.h dictionary.cpp
        doc.h doc.cpp
        crc.h crc.cpp 
        cswam.h cswam.cpp 
)

ADD_LIBRARY(cswal STATIC ${LIB_CSWA_SRC})
LINK_DIRECTORIES (${LIBRARY_OUTPUT_PATH})

FOREACH(CMD cswa)

ADD_EXECUTABLE(${CMD} ${CMD}.cpp)
TARGET_LINK_LIBRARIES (${CMD} cswal -lm -lz -lpthread)

ENDFOREACH()

#INSTALL INCLUDE FILES
FILE(GLOB includes src *.h)
INSTALL(FILES ${includes} DESTINATION include)
