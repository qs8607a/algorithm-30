# Distributed under the Boost Software License, Version 1.0.
# See http://www.boost.org/LICENSE_1_0.txt

include(Ryppl)
include(RypplDocumentation)
include(RypplQuickbook)
include(RypplDoxygen)

ryppl_find_and_use_package(Boostbook)
ryppl_find_and_use_package(Quickbook)



set (AlgoHeaders
  ../include/boost/algorithm/clamp.hpp
  ../include/boost/algorithm/hex.hpp
  ../include/boost/algorithm/minmax_element.hpp
  ../include/boost/algorithm/minmax.hpp

  ../include/boost/algorithm/searching/boyer_moore_horspool.hpp
  ../include/boost/algorithm/searching/boyer_moore.hpp
  ../include/boost/algorithm/searching/knuth_morris_pratt.hpp
)

ryppl_doxygen(algo_doxy
  XML
  INPUT
    ${AlgoHeaders}
)


# convert from doxygen XML to to boostbook XML
xsltproc(
  INPUT
    ${algo_doxy_xml}
  OUTPUT
    ${CMAKE_CURRENT_BINARY_DIR}/autodoc.xml
  CATALOG
    ${BOOSTBOOK_CATALOG}
  STYLESHEET
    ${BOOSTBOOK_XSL_DIR}/doxygen/doxygen2boostbook.xsl
)

set(dbk_file ${CMAKE_CURRENT_BINARY_DIR}/algo.dbk)



ryppl_quickbook(${CMAKE_CURRENT_SOURCE_DIR}/algorithm.qbk algorithm.xml
  INCLUDES ${CMAKE_CURRENT_SOURCE_DIR}
)

set(dbk_file ${CMAKE_CURRENT_BINARY_DIR}/algo.dbk)

xsltproc(
  INPUT
    ${CMAKE_CURRENT_BINARY_DIR}/algorithm.xml
  OUTPUT
    ${dbk_file}
  CATALOG
    ${BOOSTBOOK_CATALOG}
  STYLESHEET
    ${BOOSTBOOK_XSL_DIR}/docbook.xsl
  DEPENDS
    ${CMAKE_CURRENT_BINARY_DIR}/autodoc.xml
  PATH
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
  )

ryppl_documentation(${dbk_file})
