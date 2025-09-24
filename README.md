# abapGitFileFormatExcerpt

This repository now serves as a compact documentary companion for anyone emitting abapGit-compatible repository artefacts. Rather than bundling runnable tooling, it curates the minimum set of references and blueprints needed to understand the disk layout, file naming, and XML payloads that abapGit expects.

## Key entry points

### Reference material
- [Object format reference](docs/object-format-reference.md) – explains required stems, sidecar files, and XML structures for the most common object types.
- [Supported object types table](supported_object_types.json) – upstream catalogue of every repository object that abapGit can currently serialize.

### Blueprint samples
Each example folder contains the minimal pair of text and XML files that abapGit stores on disk. The XML artefacts are the primary entry points when adapting payloads for generated content.

- CDS view: [`zdemo_view.ddls.xml`](examples/cds_view/zdemo_view.ddls.xml)
- ABAP class: [`zcl_demo_class.clas.xml`](examples/class/zcl_demo_class.clas.xml)
- ABAP interface: [`zif_demo_interface.intf.xml`](examples/interface/zif_demo_interface.intf.xml)
- Executable program: [`z_demo_report.prog.xml`](examples/program/z_demo_report.prog.xml)
- Function group: [`zfg_demo.fugr.xml`](examples/function_group/zfg_demo.fugr.xml)
- Data dictionary artefacts:
  - Domain: [`zdemo_domain.doma.xml`](examples/domain/zdemo_domain.doma.xml)
  - Data element: [`zdemo_element.dtel.xml`](examples/data_element/zdemo_element.dtel.xml)
  - Table: [`zdemo_table.tabl.xml`](examples/table/zdemo_table.tabl.xml)
  - Search help: [`zdemo_search.shlp.xml`](examples/search_help/zdemo_search.shlp.xml)
  - Message class: [`zdemo_messages.msag.xml`](examples/message_class/zdemo_messages.msag.xml)
  - Package descriptor: [`package.devc.xml`](examples/package/package.devc.xml)

## How to use these materials
1. Start with the reference guide to confirm which files and XML segments your generator must emit for the target object type.
2. Open the matching blueprint XML to inspect the canonical structure, then adapt the textual sidecar (for example the `.abap` implementation) to your scenario.
3. Cross-check the object type identifier against `supported_object_types.json` whenever you introduce a new artefact to ensure abapGit has first-class support.

## Provenance
Source snippets and structure conventions originate from the community project https://github.com/abapGit/abapGit and remain subject to the same license terms.
