# ZEIT ONLINE Sysdig scan action

---

**_NOTE:_** This Action is used internally by the ZEIT ONLINE organization and is probably not useful outside of this specific context.

---

## Summary

This composite action is used to scan Docker images for vulnerabilities and integrate with Sysdig secure. The secure token needed
for this is fetched from Vault. In ZEIT ONLINE building workflows the permission for this is provided by
the [Baseproject action](https://github.com/ZeitOnline/gh-action-baseproject/).

The action assumes a locally built image that is subsequently scanned.

## Example Usage

Scan image for vulnerabilities:

```yaml
jobs:
    build:
        # ...
        steps:
            # ...
            - name: Scan image
              uses: ZeitOnline/sysdig-scan-action@v1.0.1
              with:
                gha_vault_role: ${{ steps.baseproject.outputs.gha_vault_role }}
                image_tag: ${{ env.REPOSITORY }}:${{ inputs.tag }}
            # ...
```

Perform IaC scan:

```yaml
jobs:
    build:
        # ...
        steps:
            # ...
            - name: Scan k8s manifests
              uses: ZeitOnline/sysdig-scan-action@v1.0.1
              with:
                gha_vault_role: ${{ steps.baseproject.outputs.gha_vault_role }}
                mode: iac
                iac_scan_path: ./k8s
                recursive: true
            # ...
```

This usage assumes a preceding step with id `baseproject` that outputs the `gha_vault_role`. This is needed to authenticate to Vault
for fetching the Sysdig secure token.

## Reference

Here are all the inputs available through `with`:

| Input                        | Description                                                                       | Default | Required |
| ---------------------------- | --------------------------------------------------------------------------------- | ------- | -------- |
| `mode`                       | Whether to scan OCI images (`vm`) or IaC files (`iac`)                            | `vm`    | ✔       |
| `gha_default_role`           | The name of the GHA default role as output by the baseproject action              |         | ✔       |
| `image_tag`                  | The name and tag of the Docker image to be scanned. Assumes locally built image   |         |          |
| `stop_on_failed_policy_eval` | Whether to fail the action when the policy evaluation fails                       | 'true'  |          |
| `iac_scan_path`              | Directory path where IaC files to be scanned reside                               |         |          |
| `recursive`                  | Whether to scan IaC files recursively                                             |         |          |

## Releases

This action uses [Release Please](https://github.com/googleapis/release-please-action). To create a new release, create a PR and use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) as described [here](https://docs.zeit.de/ops/terraform-infra/terraform/repos.html#modulversionierung).
