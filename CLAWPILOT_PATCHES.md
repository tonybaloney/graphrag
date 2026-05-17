# Clawpilot Patches

This branch (`clawpilot-patches`) carries downstream patches used by the
[Clawpilot Dreaming](https://github.com/anthonyshaw_microsoft/m) feature.

## Patches

### 1. Content-policy skip in derive_from_rows

Azure OpenAI / Foundry models occasionally flag harmless personal content
(names in calendar entries, internal project codenames) as ContentPolicyViolation.
The upstream behaviour is to abort the entire parallel transformation on the
first such failure, which makes the dreaming pipeline unusable for many
tenants.

This branch downgrades content-policy violations to per-document warnings
(`logger.warning` + skip) while still aborting on any other exception.
See `packages/graphrag/graphrag/index/utils/derive_from_rows.py`.

### 2. litellm bumped to 1.85.0

Upstream pins `litellm==1.82.6`. We bump to `1.85.0` (latest stable as of
the patch date) to pick up upstream provider fixes (notably the Azure
`api_version` handling that affects the Foundry endpoint we target).
See `packages/graphrag-llm/pyproject.toml`.

## Maintenance

This branch is rebased onto `main` periodically. Tag releases as
`clawpilot-vX.Y.Z` corresponding to the upstream version they're rebased
onto, so Clawpilot's `requirements.txt` can pin to a specific tag rather
than the rolling branch tip.
