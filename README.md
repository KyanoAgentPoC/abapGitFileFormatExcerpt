# abapGitFileFormatExcerpt

This repository packages a focused set of files copied from the community abapGit project so agentic ABAP development tools have a concrete reference for how repository objects are structured on disk.

The primary intent is to accelerate the creation of new ABAP objects that will be synchronized through abapGit. By reusing these examples, an agent can emit correctly named files, folders, and metadata so the generated objects round-trip cleanly between the ABAP system and the Git repository.

## How to use this repository
- Inspect the sample objects to understand the expected file layout, metadata, and naming conventions abapGit relies on.
- Use the patterns as templates when programmatically generating new ABAP development objects for abapGit synchronization.
- Keep the excerpts read-only unless you purposely want to expand the catalog with more reference objects.

## Source
All excerpts originate from https://github.com/abapGit/abapGit and remain subject to the same license terms.
