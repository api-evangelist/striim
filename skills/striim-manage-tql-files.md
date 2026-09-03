---
name: striim-manage-tql-files
description: Upload, list, and delete TQL files on a Striim cluster (new in Striim 5.4.0.2).
api: Striim TQL Files REST API (5.4.0.2, base path /api/v2 on your Striim server or Striim Cloud service)
operations:
  - TqlFilesUpload
  - TqlFilesList
  - TqlFilesDelete
generated: 2026-09-03
method: generated
source: openapi/striim-tql-files-rest-api-5-4-0-2-openapi.yml
---

# Manage TQL files

TQL (Tungsten Query Language) files define Striim applications as code. These endpoints are new in
Striim 5.4.0.2. All calls require `Authorization: STRIIM-TOKEN <36-character token>`.

1. **Upload.** `POST /tqlfiles` (`TqlFilesUpload`) uploads a TQL file to the authenticated user's
   home directory on the Striim cluster.
2. **List.** `GET /tqlfiles` (`TqlFilesList`) returns every TQL file in the user's upload directory
   with name, size and lastModified.
3. **Delete.** `DELETE /tqlfiles/{name}` (`TqlFilesDelete`) removes the named file — the documented
   reversal of upload.

To turn an uploaded TQL file into a running application, execute its DDL with `POST /tungsten`
(see the striim-execute-console-commands skill), then deploy and start it.
