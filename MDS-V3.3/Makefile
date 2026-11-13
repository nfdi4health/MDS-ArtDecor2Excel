# Build the NFDI4Health Metadata Schema (MDS) v3.3 as Excel-compatible CSVs.
#
# MDS v3.3 only consisted of Core + Design (the RecordLinkage /
# NutritionalEpidemiology / ChronicDiseases modules did not exist yet).
#
# The authoritative v3.3 data lives in this directory (CoreMDS33.xml and
# Design33.xml), because ART-DECOR currently no longer serves the v3.3
# snapshots reliably (its RetrieveTransaction / $extract services time out
# server-side for the historical v3.3 effective dates, and the plain /dataset
# endpoint ignores the historical date and returns the current version). So
# this makefile transforms those already-downloaded local XMLs instead of
# re-fetching them.
#
# It reuses the shared XSL stylesheet (../ArtDecor2Excel.xsl) and the shared
# Saxon/xmlresolver jars in the repo root (../), which the root Makefile
# fetches. Run from the repo root:
#     make -C MDS-V3.3                  # build the v3.3 CSVs
#     make -C MDS-V3.3 clean            # remove the generated CSVs
#     make -C MDS-V3.3 distclean        # also remove the shared jars
# Version 13.11.2026

XSL     := ../ArtDecor2Excel.xsl
# Space-separated jar prerequisites (make prerequisites must use spaces).
JAR_DEPS := ../Saxon-HE-13.1.jar ../xmlresolver-6.1.1.jar
# Colon-separated Java classpath (used only in the java -cp invocation).
JARS_CP := ../Saxon-HE-13.1.jar:../xmlresolver-6.1.1.jar

CSV_FILES = MDS_Core_V3_3.csv MDS_Design_V3_3.csv
COMBINED  = MDS_Core_Design_V3_3.csv MDS_V3_3.csv

all: $(COMBINED) $(CSV_FILES)

# Every file this makefile creates (CSVs), plus the shared jars fetched into ../.
CLEAN_FILES   = $(CSV_FILES) $(COMBINED)
DISTCLEAN     = $(CLEAN_FILES) $(JAR_DEPS)

clean:
	rm -f $(CLEAN_FILES)

distclean: clean
	rm -f $(JAR_DEPS)

# ----- fetch the shared Saxon/xmlresolver jars into the repo root -----
../Saxon-HE-13.1.jar:
	mvn dependency:copy -Dartifact=net.sf.saxon:Saxon-HE:13.1 -DoutputDirectory=..

../xmlresolver-6.1.1.jar:
	mvn dependency:copy -Dartifact=org.xmlresolver:xmlresolver:6.1.1 -DoutputDirectory=..

# ----- transform local v3.3 XML -> CSV (Excel-compatible) -----
MDS_Core_V3_3.csv: CoreMDS33.xml $(XSL) $(JAR_DEPS)
	java -cp "$(JARS_CP)" net.sf.saxon.Transform -xsl:$(XSL) -s:$< -o:$@

MDS_Design_V3_3.csv: Design33.xml $(XSL) $(JAR_DEPS)
	java -cp "$(JARS_CP)" net.sf.saxon.Transform -xsl:$(XSL) -s:$< -o:$@

# ----- combined CSVs -----
MDS_Core_Design_V3_3.csv: $(CSV_FILES)
# print column headers
	head -n 1 MDS_Core_V3_3.csv > $@
# print rows
	for file in $(CSV_FILES); do \
		tail -n+2 $$file && echo ""; \
	done >> $@

MDS_V3_3.csv: $(CSV_FILES)
# print column headers
	head -n 1 MDS_Core_V3_3.csv > $@
# print rows
	for file in $(CSV_FILES); do \
		tail -n+2 $$file && echo ""; \
	done >> $@
