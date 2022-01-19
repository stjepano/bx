
file(GLOB BX_HEADER_FILES "./include/bx/*.h" "./include/bx/inline/*.inl")
file(GLOB BX_SRC_FILES "./src/*.cpp")
list(FILTER BX_SRC_FILES EXCLUDE REGEX ".*src/amalgamated\\.cpp$")
list(FILTER BX_SRC_FILES EXCLUDE REGEX ".*src/crtnone\\.cpp$")
#message(STATUS ${BX_SRC_FILES})

set(TGT "bx")

add_library(${TGT} STATIC ${BX_HEADER_FILES} ${BX_SRC_FILES})

set(COMPAT_INCLUDE_DIR "./include/compat/msvc")
set(BX_DEFS "")

if (MSVC)
	set(COMPAT_INCLUDE_DIR "./include/compat/msvc")
	set(BX_DEFS "${BX_DEFS} _CRT_SECURE_NO_WARNINGS -D__STDC_FORMAT_MACROS")
endif()

if (CMAKE_BUILD_TYPE MATCHES DEBUG)
	set(BX_DEFS "${BX_DEFS} -DBX_CONFIG_DEBUG=1")
else ()
	set(BX_DEFS "${BX_DEFS} -DBX_CONFIG_DEBUG=0")
endif()

target_include_directories(${TGT} PRIVATE "./3rdparty" "./include" ${COMPAT_INCLUDE_DIR})
target_compile_definitions(${TGT} PRIVATE ${BX_DEFS})

