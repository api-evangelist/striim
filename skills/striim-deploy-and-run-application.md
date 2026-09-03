---
name: striim-deploy-and-run-application
description: Create a Striim application from a template, deploy it, start it, and monitor it — with the documented reversal at every step.
api: Striim Application Management REST API (5.4.0.2, base path /api/v2 on your Striim server or Striim Cloud service)
operations:
  - ApplicationsTemplatesGet
  - ApplicationsTemplatesByTemplateIdGet
  - ApplicationsPost
  - ApplicationsDeploymentByAppFqnPost
  - ApplicationsSprintByAppFqnPost
  - ApplicationsApplicationMonitoringReportByApplicationNameGet
  - ApplicationsSprintByAppFqnDelete
  - ApplicationsDeploymentByAppFqnDelete
  - ApplicationsByAppFqnDelete
generated: 2026-09-03
method: generated
source: openapi/striim-tql-files-rest-api-5-4-0-2-openapi.yml
---

# Deploy and run a Striim application

All calls go to `http(s)://<your-striim-host>:9080/api/v2` and require the header
`Authorization: STRIIM-TOKEN <36-character token>` (see authentication/striim-authentication.yml).
Applications are addressed by fully qualified name `<namespace>.<application>` (`appFqn`).

1. **Pick a template.** `GET /applications/templates` (`ApplicationsTemplatesGet`) lists templates;
   `GET /applications/templates/{templateId}` (`ApplicationsTemplatesByTemplateIdGet`) returns each
   property's type, default value and whether it is required.
2. **Create the application.** `POST /applications` (`ApplicationsPost`) with `templateId` (or a
   `templateDefinition`), `applicationName`, `applicationSettings` (recovery, encryption,
   exceptionHandlers), and source/target/parser/formatter parameters.
3. **Deploy.** `POST /applications/{appFqn}/deployment` (`ApplicationsDeploymentByAppFqnPost`).
   The optional AppDeploymentPlan names deployment groups per flow.
4. **Start.** `POST /applications/{appFqn}/sprint` (`ApplicationsSprintByAppFqnPost`) starts the
   deployed application.
5. **Monitor.** `GET /applications/monitoring/report/{applicationnameOrComponentName}`
   (`ApplicationsApplicationMonitoringReportByApplicationNameGet`); the application object's
   `links[]` (rel/allow/href) advertises which transitions are currently allowed.

Reversals (documented, no stated windows): stop with `DELETE .../sprint`
(`ApplicationsSprintByAppFqnDelete`), undeploy with `DELETE .../deployment`
(`ApplicationsDeploymentByAppFqnDelete`), drop the application and all its components with
`DELETE /applications/{appFqn}` (`ApplicationsByAppFqnDelete`).

Errors: HTTP 412 means the application is not in a state that allows the transition (for example
starting an undeployed application); 401 means the STRIIM-TOKEN is missing or invalid. Errors are
plain JSON (see errors/striim-problem-types.yml). No idempotency keys and no rate-limit headers are
documented — retry state transitions only after re-reading application state.
