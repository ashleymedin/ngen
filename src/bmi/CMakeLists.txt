add_library(ngen_bmi)
add_library(NGen::ngen_bmi ALIAS ngen_bmi)

target_include_directories(ngen_bmi
  PUBLIC
    ${NGEN_INC_DIR}
)

target_link_libraries(ngen_bmi
  PUBLIC
    ${CMAKE_DL_LIBS}
    Boost::boost # Headers-only Boost
    NGen::config_header
    NGen::logging
    NGen::geojson
    NGen::core_mediator
)

target_sources(ngen_bmi
  PRIVATE
    "${CMAKE_CURRENT_LIST_DIR}/Bmi_Adapter.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/AbstractCLibBmiAdapter.cpp"
    "${CMAKE_CURRENT_LIST_DIR}/Bmi_Cpp_Adapter.cpp"
)

if(NGEN_WITH_BMI_C)
    add_custom_target(announce_ngen_bmi_c
      COMMAND ${CMAKE_COMMAND} -E echo "Building ngen_bmi: C BMI support enabled"
      COMMENT "Announce C BMI build"
    )
    target_sources(ngen_bmi PRIVATE "${CMAKE_CURRENT_LIST_DIR}/Bmi_C_Adapter.cpp")
    add_dependencies(ngen_bmi announce_ngen_bmi_c)
endif()

if(NGEN_WITH_PYTHON)
    add_custom_target(announce_ngen_bmi_py
      COMMAND ${CMAKE_COMMAND} -E echo "Building ngen_bmi: PY BMI support enabled"
      COMMENT "Announce PY BMI build"
    )
    target_sources(ngen_bmi PRIVATE "${CMAKE_CURRENT_LIST_DIR}/Bmi_Py_Adapter.cpp")
    target_link_libraries(ngen_bmi PUBLIC pybind11::embed)
    add_dependencies(ngen_bmi announce_ngen_bmi_py)
endif()

if(NGEN_WITH_BMI_FORTRAN)
    add_custom_target(announce_ngen_bmi_fortran
      COMMAND ${CMAKE_COMMAND} -E echo "Building ngen_bmi: Fortran BMI support enabled"
      COMMENT "Announce Fortran BMI build"
    )
    target_sources(ngen_bmi PRIVATE "${CMAKE_CURRENT_LIST_DIR}/Bmi_Fortran_Adapter.cpp")
    target_link_libraries(ngen_bmi PUBLIC iso_c_bmi)
    add_dependencies(ngen_bmi announce_ngen_bmi_fortran)
endif()
