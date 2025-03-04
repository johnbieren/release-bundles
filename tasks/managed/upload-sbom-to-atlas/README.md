# upload-sbom-to-atlas
Tekton task is gathering SBOM data form working directory, converting it to supported version if needed and uploading it to Atlas.
Supports both CycloneDX and SPDX format.

## Parameters

| Name                      | Description                                                                                                                                                                                                                          | Optional | Default value                                                                 |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------------------------|
| sbomDir                   | Directory containing SBOM files. The task will search for CycloneDX JSON SBOMs recursively in this directory and upload them all to Atlas. The path is relative to the 'data' workspace.                                             | No       | None                                                                          |
| httpRetries               | Max HTTP retry count.                                                                                                                                                                                                                | Yes      | 3                                                                             |
| atlasSecretName           | Name of the Secret containing SSO auth credentials for Atlas.                                                                                                                                                                        | Yes      | atlas-prod-sso-secret                                                         |
| bombasticApiUrl           | URL of the BOMbastic API host of Atlas.                                                                                                                                                                                              | Yes      | https://sbom.atlas.devshift.net                                               |
| ssoTokenUrl               | URL of the SSO token issuer.                                                                                                                                                                                                         | Yes      | https://auth.redhat.com/auth/realms/EmployeeIDP/protocol/openid-connect/token |
| supportedCycloneDxVersion | If the SBOM uses a higher CycloneDX version, `syft convert` in the task will convert all SBOMs to this CycloneDX version before uploading them to Atlas. If the SBOM is already in this version or lower, it will be uploaded as is. | Yes      | 1.4                                                                           |
| supportedSpdxVersion      | If the SBOM uses a higher SPDX version, `syft convert` in the task will convert all SBOMs to this SPDX version before uploading them to Atlas. If the SBOM is already in this version or lower, it will be uploaded as is.           | Yes      | 2.3                                                                           |

## Changes in 0.2.1
Ignore error (but output a message) if upload request to Atlas fails.

## Changes in 0.2.0
Remove option to skip uploading SBOMs. Skipping will be handled via Tekton.
Rename productSBOMPath parameter to sbomDir. Use SBOM file names as Atlas IDs.
