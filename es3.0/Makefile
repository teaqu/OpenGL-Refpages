# Copyright (c) 2013-2017 The Khronos Group Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# XML validation tool
JING = jing

default clean:
	cd html ; $(MAKE) $@

XIFILES = compressedformattable.xml \
	  internalformattable.xml \
	  renderbufferformattable.xml \
	  unsizedformattable.xml \
	  apifunchead.xml \
	  apiversion.xml \
	  funchead.xml \
	  varhead.xml \
	  version.xml

# Docbook 5 Relax-NG with XInclude schema URLs and local filenames
DB5XIRNCURL = http://docbook.org/xml/5.0/rng/docbookxi.rnc
DB5XIRNC    = /usr/share/xml/docbook/schema/rng/5.0/docbookxi.rnc

XML = $(filter-out $(XIFILES),$(wildcard *.xml))

validate:
	jing -c $(DB5XIRNC) $(XML)

FO_STYLESHEET = /usr/share/xml/docbook/stylesheet/docbook-xsl/fo/docbook.xsl

# Target to generate individual PDFs and then merge them
pdf: OpenGL_ES_Reference_Pages.pdf

# Rule to generate .fo files from .xml files
%.fo: %.xml
	xsltproc --nonet --xinclude -o $@ $(FO_STYLESHEET) $<

# Rule to generate .pdf files from .fo files
%.pdf: %.fo
	fop $< $@

# Rule to generate all individual PDFs
individual_pdfs: $(XML:.xml=.pdf)

# Rule to merge PDFs into one
OpenGL_ES_Reference_Pages.pdf: individual_pdfs
	@echo "Merging PDFs into OpenGL_ES_Reference_Pages.pdf"
	# Ensure pdfunite is installed or replace with an available tool
	pdfunite $(sort $(XML:.xml=.pdf)) $@

# Clean up generated files
clean:
	find . -type f \( -name '*.fo' -o -name '*.pdf' ! -name 'OpenGL_ES_Reference_Pages.pdf' \) -delete