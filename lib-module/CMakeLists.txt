# ================ Library ================
set(INCLUDES_DIR "${PROJECT_SOURCE_DIR}/include")
set(LIBRARY_HEADERS ${INCLUDES_DIR}/)
set(TARGET mylib)

add_library(${TARGET})
target_sources(${TARGET}
    PRIVATE MyLib.cpp
    PUBLIC  ${LIBRARY_HEADERS}
)

target_include_directories(${TARGET} PUBLIC ${LIBRARY_HEADERS})
target_link_libraries(${TARGET} PRIVATE)
target_compile_features(${TARGET} PUBLIC cxx_std_20)

source_group(
    TREE "${INCLUDES_DIR}"
    PREFIX "Header Files"
    FILES ${FILE_LIBRARY_HEADERS})
