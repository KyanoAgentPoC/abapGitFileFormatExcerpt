# abapGit Object File Reference

This guide captures the minimal file layout agentic tools should emit when generating new ABAP development objects for repositories synchronized with abapGit. Each section lists the file naming scheme the abapGit serializer expects plus a tiny payload example to keep size low while preserving the final on-disk structure.

## Naming Rules Shared By All Objects
- File names follow the pattern `<object name>.<object type specific suffix>` (e.g. `zcl_demo_class.clas.abap`).
- Every code artefact has a sibling XML metadata file with the same stem (e.g. `zcl_demo_class.clas.xml`).
- XML files must be UTF-8 encoded and wrap the SAP data cluster in the `asx:abap` envelope.
- abapGit consumes lowercase file extensions; keep the object name uppercase as in SAP.

## Global Class (`CLAS`)
```
objects/
  zcl_demo_class.clas.abap
  zcl_demo_class.clas.xml
```
- **ABAP file**: Include `CLASS ... DEFINITION` and `CLASS ... IMPLEMENTATION` blocks. Keep methods empty if you only need a placeholder.
- **XML file**: Minimal sample body:
  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <abapGit version="v1.0.0" serializer="LCL_OBJECT_CLAS" serializer_version="v1.0.0">
    <asx:abap xmlns:asx="http://www.sap.com/abapxml" version="1.0">
      <asx:values>
        <VSEOCLASS>
          <CLSNAME>ZCL_DEMO_CLASS</CLSNAME>
          <LANGU>E</LANGU>
          <STATE>1</STATE>
        </VSEOCLASS>
      </asx:values>
    </asx:abap>
  </abapGit>
  ```
- Optional includes (locals, macros, testclasses) become extra files named `zcl_demo_class.clas.locals_imp.abap` etc.
- Text symbols are emitted in the companion XML inside the `<TPOOL>` block (see the dedicated section below).

## Interface (`INTF`)
```
objects/
  zif_demo_interface.intf.abap
  zif_demo_interface.intf.xml
```
- ABAP file should contain a basic `INTERFACE ...` definition.
- XML metadata wraps `VSEOINTERF` with key fields `INTFNAME`, `LANGU`, `STATE`.

## Executable Program (`PROG`)
```
objects/
  z_demo_report.prog.abap
  z_demo_report.prog.xml
```
- ABAP program files are flat source reports; the XML encapsulates the `REPOSITORY_OBJECT` structure (`PROGDIR`, `TPOOL`).
- Include any text elements, selection texts, and headings as `<TPOOL>` entries so the program receives the correct text pool when imported.

## Function Group (`FUGR`)
```
objects/
  zfg_demo.fugr.xml
  zfg_demo.fugr.lzfg_demo_top.abap
  zfg_demo.fugr.saplzfg_demo.abap
```
- Function pools split across TOP include, main include, and function include files named `L`/`SAPL` + group.
- XML lists metadata nodes `TLIBG` (attributes) and `LIMU` entries for released function modules.
- Text pools for the includes live in the same XML under `<TPOOL>`.

## Package (`DEVC`)
```
objects/
  package.devc.xml
```
- Packages are metadata-only; XML holds `TDEVC` with `DEVCLASS`, `CTEXT`, `DLVUNIT` and transport settings.

## Domain (`DOMA`)
```
objects/
  zdemo_domain.doma.abap
  zdemo_domain.doma.xml
```
- ABAP stub is optional; XML describes `DD01V`/`DD07V` tables to carry data type and fixed values.

## Data Element (`DTEL`)
```
objects/
  zdemo_element.dtel.abap
  zdemo_element.dtel.xml
```
- XML uses `DD04V` and, for search help binding, `DD05M` sections. The ABAP file contains the generated documentation (often blank).

## CDS View (`DDLS`)
```
objects/
  zdemo_view.ddls.asddl
  zdemo_view.ddls.xml
```
- Use `.asddl` for the DDL source and `.xml` to store activation metadata from `DDLHEADER`.

## Message Class (`MSAG`)
```
objects/
  zdemo_messages.msag.xml
```
- Message classes are metadata-only. XML hosts `T100A` (header) and repeated `T100` records for individual messages.

## Search Help (`SHLP`)
```
objects/
  zdemo_search.shlp.xml
```
- Search help XML combines `DD30V` (header) plus `DD31V`/`DD32V` child tables describing parameters and selection methods.

## Database Table (`TABL`)
```
objects/
  zdemo_table.tabl.xml
```
- abapGit exports table definitions solely as XML built around `DD02V` (header) and `DD03P` rows (fields). Technical settings live in `DD09L`.

## Text Symbols (`TPOOL`)
- abapGit serializes text symbols by copying the SAP text pool entries into the object's XML under `<TPOOL>`.
- Each symbol appears as a child `<TPOOL>` node with the native fields: `ID` (type, e.g. `I` for text element or `R` for selection text), `KEY` (text ID or selection name), `ENTRY` (the text itself), plus `LANGU`, `LENGTH`, etc. when available.
- To add or change text symbols, edit these entries in the XML; upon import abapGit calls the standard `INSERT TEXTPOOL` APIs so the ABAP runtime sees the updated texts.
- Provide separate `<TPOOL>` sections for every language you need. abapGit will overwrite the existing pool for the same object/language during deserialization.
- Programs, class pools, and function groups all use this mechanism, so text maintenance stays consistent across object types.
- Sample implementations in this repository: `examples/class/zcl_demo_class.clas.xml`, `examples/program/z_demo_report.prog.xml`, and `examples/function_group/zfg_demo.fugr.xml` show English and German text entries.

## General Tips
- Always include transport layer and package assignments in XML when available (`DEVCLASS`, `DLVUNIT`).
- Keep the repository arranged exactly as `abapGit` produces it: one folder (commonly `objects/`) containing the serialized artefacts.
- When you only need to signal the presence of an object type, prefer metadata-only XML to avoid shipping extensive ABAP logic.
- The `supported_object_types.json` file in the repository root enumerates the object types abapGit can serialize; use it to decide which sections to emit.
