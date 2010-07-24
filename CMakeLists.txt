#
# Copyright Troy D. Straszheim
#
# Distributed under the Boost Software License, Version 1.0.
# See http://www.boost.org/LICENSE_1_0.txt
#
#----------------------------------------------------------------------------

# Add a library target to the build system
ryppl_library_project(
  boost.algorithm
  #  SRCDIRS 
  TESTDIRS  minmax/test string/test
  EXAMPLEDIRS minmax/example string/example
  #  DOCDIRS 
  DESCRIPTION  "A library of various algorithms."
  AUTHORS  "Pavol Droba <droba -at- topmail.sk>"
           "Herve Bronnimann <hbr -at- poly.edu>"
  #  MAINTAINERS 
VERSION 1.41.0)