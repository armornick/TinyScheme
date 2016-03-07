cmake_minimum_required (VERSION 3.1)
project (stlplus)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# ----------------------------------------------------------------------------

set (TINYSCHEME_DIR "tinyscheme-1.41")

# ----------------------------------------------------------------------------

set (TINYSCHEME_SRCS
	${TINYSCHEME_DIR}/scheme.c
	${TINYSCHEME_DIR}/dynload.c
)

if (WIN32)

	set(TINYSCHEME_DEFS
		USE_STRLWR=0 USE_DL=1 USE_MATH=1 USE_ASCII_NAMES=0
	)

if (MSVC)
	set(TINYSCHEME_DEFS ${TINYSCHEME_DEFS} _CRT_SECURE_NO_WARNINGS _CRT_NONSTDC_NO_WARNINGS)
endif(MSVC)

else(WIN32)
	set(TINYSCHEME_DEFS
		SUN_DL=1 USE_DL=1 USE_MATH=1 USE_ASCII_NAMES=0
	)
endif(WIN32)

# ----------------------------------------------------------------------------

add_library (tinyscheme SHARED ${TINYSCHEME_SRCS})
target_compile_definitions(tinyscheme PUBLIC ${TINYSCHEME_DEFS})

add_executable(tinyscheme-app ${TINYSCHEME_SRCS})
target_compile_definitions(tinyscheme-app PUBLIC ${TINYSCHEME_DEFS} STANDALONE)
set_target_properties (tinyscheme-app PROPERTIES OUTPUT_NAME tinyscheme)

# ----------------------------------------------------------------------------
