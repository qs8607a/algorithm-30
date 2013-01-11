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

set(doxy_xml ${CMAKE_CURRENT_BINARY_DIR}/algo_doxy.xml)
ryppl_doxygen(algo_doxy
  XML
  INPUT
    ${AlgoHeaders}
  OUTPUT
    ${doxy_xml}
)


# convert from doxygen XML to to boostbook XML
xslt_doxy_to_boostbook(
  INPUT
    ${doxy_xml}
  OUTPUT
    ${CMAKE_CURRENT_BINARY_DIR}/autodoc.xml
)


quickbook_to_boostbook(${CMAKE_CURRENT_SOURCE_DIR}/algorithm.qbk algorithm.xml
  INCLUDES ${CMAKE_CURRENT_SOURCE_DIR}
)

set(dbk_file ${CMAKE_CURRENT_BINARY_DIR}/algo.dbk)

xslt_boostbook_to_docbook(
  INPUT
    ${CMAKE_CURRENT_BINARY_DIR}/algorithm.xml
  OUTPUT
    ${dbk_file}
  DEPENDS
    ${CMAKE_CURRENT_BINARY_DIR}/autodoc.xml
  PATH
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
)

set(HtmlParameters
  boost.root=../../../..
  boost.doxygen.reftitle="Boost.Algorithms C++ Reference"
  chapter.autolabel=0
  chunk.section.depth=8
  toc.section.depth=2
  toc.max.depth=2
  generate.section.toc.level=1
)


set(html_dir "${CMAKE_CURRENT_BINARY_DIR}/html")
set(manifest_file ${html_dir}/algo_HTML.manifest)

xslt_docbook_to_html(
  INPUT
    ${dbk_file}
  OUTPUT
    ${html_dir}
  PARAMETERS
    ${HtmlParameters}
  MANIFEST
    ${manifest_file}
)

add_to_doc(${manifest_file} html)
