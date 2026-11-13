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

Version 13.11.2026 © F. Meineke for NFDI4Health

ART-DECOR® is a registered trademark.
