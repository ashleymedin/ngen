include(${PROJECT_SOURCE_DIR}/cmake/dynamic_sourced_library.cmake)
dynamic_sourced_cxx_library(realizations_catchment "${CMAKE_CURRENT_SOURCE_DIR}")

add_library(NGen::realizations_catchment ALIAS realizations_catchment)

target_include_directories(realizations_catchment PUBLIC
        ${PROJECT_SOURCE_DIR}/include/core
        ${PROJECT_SOURCE_DIR}/include/core/catchment
        ${PROJECT_SOURCE_DIR}/include/realizations/catchment
        ${PROJECT_SOURCE_DIR}/include/forcing
        ${PROJECT_SOURCE_DIR}/include/simulation_time
        ${PROJECT_SOURCE_DIR}/include/utilities
        ${PROJECT_SOURCE_DIR}/include/geojson
        ${PROJECT_SOURCE_DIR}/include/bmi
        )
        
target_link_libraries(realizations_catchment PUBLIC
        ${CMAKE_DL_LIBS}
        NGen::config_header
        NGen::core_catchment
        NGen::geojson
        NGen::logging
        NGen::ngen_bmi
        )

add_custom_target(announce_realizations_catchment
  COMMAND ${CMAKE_COMMAND} -E echo "Building realizations_catchment"
  COMMENT "Announce realizations_catchment build"
)
add_dependencies(realizations_catchment announce_realizations_catchment)
