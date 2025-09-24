# abapGitFileFormatExcerpt

This repository distills the essentials an agent needs to emit abapGit-compatible repository objects. Instead of shipping the whole abapGit implementation, it now contains:
- Concise format notes in `docs/object-format-reference.md` describing file stems, required XML payloads, and optional includes for common object types.
- Lightweight sample artefacts under `examples/` that show how a minimal class, interface, program, function group, and other dictionary objects serialize to disk.
- The upstream `supported_object_types.json` table to cross-check which SAP repository objects abapGit can serialize.

## How to use this repository
- Start with the reference guide to learn which files you must emit for a given object type.
- Copy the stubs in `examples/` when you need concrete files to feed into abapGit or extend them with your generated logic.
- Treat the samples as blueprints: keep the file names, extensions, and XML scaffolding intact while swapping in your content.

## Source
Source snippets and structure conventions originate from the community project https://github.com/abapGit/abapGit and remain subject to the same license terms.

