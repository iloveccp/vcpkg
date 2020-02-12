# Lua can be compiled as either C or C++.
# Default configuration is C, set COMPILE_AS_CPP to ON to use C++.
# See http://stackoverflow.com/questions/13560945/c-and-c-library-using-longjmp for why would you want to do that.
# Primary differences:
# - Exceptions will be used instead of setjmp/longjmp
# - The name mangling for functions will be C++ instead of C.
#     - This is a source-incompatible change because extern "C" is chosen by the including application.
# - The lua.hpp header will not be available.

PROJECT ( pb )

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

if(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /TP")
endif()

SET (HDR_LIBPBC pb.h)
SET (SRC_LIBPBC pb.c)

FIND_PACKAGE(Lua REQUIRED)

INCLUDE_DIRECTORIES(${LUA_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR})

ADD_LIBRARY ( pb ${SRC_LIBPBC} ${HDR_LIBPBC})
INSTALL ( TARGETS pb
    RUNTIME DESTINATION ${CMAKE_INSTALL_PREFIX}/bin
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
    ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
)
TARGET_LINK_LIBRARIES(pb ${LUA_LIBRARIES})
INSTALL(FILES ${HDR_LIBPBC} DESTINATION include)