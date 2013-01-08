# Distributed under the Boost Software License, Version 1.0.
# See http://www.boost.org/LICENSE_1_0.txt

include(Ryppl)
include(RypplDocumentation)
include(RypplQuickbook)
include(RypplDoxygen)

ryppl_find_and_use_package(Boostbook)
ryppl_find_and_use_package(Quickbook)


ryppl_quickbook(${CMAKE_CURRENT_SOURCE_DIR}/algorithm.qbk algorithm.xml INCLUDES ${CMAKE_CURRENT_SOURCE_DIR})

set(dbk_file ${CMAKE_CURRENT_BINARY_DIR}/algo.dbk)

xsltproc(
  INPUT      ${CMAKE_CURRENT_BINARY_DIR}/algorithm.xml
  OUTPUT     ${dbk_file}
  CATALOG    ${BOOSTBOOK_CATALOG}
  STYLESHEET ${BOOSTBOOK_XSL_DIR}/docbook.xsl
  )

ryppl_documentation(${dbk_file})
