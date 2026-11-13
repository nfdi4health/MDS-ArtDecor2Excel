# MDS-ArtDecor2Excel

Transform NFDI4Health Metadataschema from ArtDecor to Excel-compatible CSV format, using XSLT.

## Prerequisites
*  mvn, make, curl and a recent java runtime, eg. `sudo apt install make maven curl default-jre`

## Install
*  clone `git clone https://github.com/nfdi4health/MDS-ArtDecor2Excel.git`
*  type `make`

This will
-   fetch needed libraries from maven
-   fetch the five MDS modules from ArtDecor (Core, Design, RecordLinkage, NutritionalEpidemiology, ChronicDiseases)
-   perform the transformations with the XSLT Stylesheet (ArtDecor2Excel.xsl)
-   create CSV files (one per module, plus the combined MDS_V3_4.csv and MDS_Core_Design_V3_4.csv), which can be opened by Excel

## MDS v3.3 (subdirectory `MDS-V3.3`)

Historical MDS v3.3 is built in its own subdirectory, `make -C MDS-V3.3`.
It reuses the same shared XSLT stylesheet (`ArtDecor2Excel.xsl`) and the
shared Saxon/xmlresolver jars from the repository root.

This will

-   fetch the shared Saxon/xmlresolver jars from maven (into the repo root) if missing
-   transform the locally stored v3.3 XMLs (`MDS-V3.3/CoreMDS33.xml` and `MDS-V3.3/Design33.xml`)
-   create `MDS_Core_V3_3.csv`, `MDS_Design_V3_3.csv`, and the combined `MDS_Core_Design_V3_3.csv` / `MDS_V3_3.csv`

MDS v3.3 only consisted of the Core and Design modules (RecordLinkage,
NutritionalEpidemiology and ChronicDiseases did not exist yet). The v3.3 data is
built from the locally stored XMLs because ART-DECOR no longer serves the v3.3
snapshots reliably (its `RetrieveTransaction`/`$extract` services time out
server-side for the historical v3.3 effective dates, and the plain `/dataset`
endpoint ignores the historical date and returns the current version).

Version 13.11.2026 © F. Meineke for NFDI4Health

ART-DECOR® is a registered trademark.
