# name of the target and the resulting library
set(PROJECT_NAME prod-lib-mdl_core)

# collect sources
set(PROJECT_SOURCES 
    "mdl_core_factory.cpp"
    )

# create target from template
create_from_base_preset(
    TARGET ${PROJECT_NAME}
    TYPE SHARED
    SOURCES ${PROJECT_SOURCES}
    EMBED_RC "mdl_core.rc"
    )

# customize name
set_target_properties(${PROJECT_NAME} PROPERTIES PREFIX "lib")
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "mdl_core")

if(MACOSX)
    set_target_properties(${PROJECT_NAME} PROPERTIES SUFFIX ".so") # corresponding to the binary release
endif()

# target alias for the custom name
add_library(mdl_core ALIAS ${PROJECT_NAME})
add_library(mdl::mdl_core ALIAS ${PROJECT_NAME})

# add mdl and other dependencies
target_add_dependencies(TARGET ${PROJECT_NAME}
    DEPENDS
        boost
        ${LINKER_WHOLE_ARCHIVE}
        ${LINKER_START_GROUP}
        mdl::include-mi
        mdl::mdl-compiler-compilercore
        mdl::mdl-codegenerators-generator_dag
        mdl::mdl-codegenerators-generator_code
        mdl::mdl-runtime
        mdl::mdl-jit-generator_jit
        mdl::mdl-no_glsl-generator_stub
        mdl::base-lib-libzip
        mdl::base-lib-zlib
        mdl::base-system-version
        ${LINKER_END_GROUP}
        llvm
        ${LINKER_NO_WHOLE_ARCHIVE}
    )
