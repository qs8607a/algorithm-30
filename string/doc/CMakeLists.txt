# Distributed under the Boost Software License, Version 1.0.
# See http://www.boost.org/LICENSE_1_0.txt

include(Ryppl)
include(RypplDocumentation)
include(RypplDoxygen)

ryppl_find_and_use_package(Boostbook)

set (StringAlgoHeaders
  ../../include/boost/algorithm/string.hpp
  ../../include/boost/algorithm/string_regex.hpp
  ../../include/boost/algorithm/string/classification.hpp
  ../../include/boost/algorithm/string/sequence_traits.hpp
  ../../include/boost/algorithm/string/std_containers_traits.hpp
  ../../include/boost/algorithm/string/concept.hpp
  ../../include/boost/algorithm/string/compare.hpp
  ../../include/boost/algorithm/string/constants.hpp
  ../../include/boost/algorithm/string/case_conv.hpp
  ../../include/boost/algorithm/string/find.hpp
  ../../include/boost/algorithm/string/finder.hpp
  ../../include/boost/algorithm/string/find_iterator.hpp
  ../../include/boost/algorithm/string/trim.hpp
  ../../include/boost/algorithm/string/predicate.hpp
  ../../include/boost/algorithm/string/split.hpp
  ../../include/boost/algorithm/string/iter_find.hpp
  ../../include/boost/algorithm/string/erase.hpp
  ../../include/boost/algorithm/string/join.hpp
  ../../include/boost/algorithm/string/replace.hpp
  ../../include/boost/algorithm/string/find_format.hpp
  ../../include/boost/algorithm/string/formatter.hpp
  ../../include/boost/algorithm/string/regex.hpp
  ../../include/boost/algorithm/string/regex_find_format.hpp
  ../../include/boost/algorithm/string/trim_all.hpp
  ../../include/boost/algorithm/string/yes_no_type.hpp
  ../../include/boost/algorithm/string/config.hpp
)

ryppl_doxygen(string_algo_doxy
  XML
  INPUT
    ${StringAlgoHeaders}
)


# convert from doxygen XML to to boostbook XML
xsltproc(
  INPUT
    ${string_algo_doxy_xml}
  OUTPUT
    ${CMAKE_CURRENT_BINARY_DIR}/autodoc.xml
  CATALOG
    ${BOOSTBOOK_CATALOG}
  STYLESHEET
    ${BOOSTBOOK_XSL_DIR}/doxygen/doxygen2boostbook.xsl
)

set(dbk_file ${CMAKE_CURRENT_BINARY_DIR}/string_algo.dbk)

xsltproc(
  INPUT
    ${CMAKE_CURRENT_SOURCE_DIR}/string_algo.xml
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
