# Umbrella bundles

An umbrella archive contains all three stacks under `deploy/stacks/` and records
independent versions in its immutable manifest. Verify the archive checksum
before extracting. Use `deploy/stacks/self-managed/` for control-plane commands
and `deploy/stacks/nvcf-compute-plane/` for compute-plane commands. Preserve the
sibling `deploy/stacks/observability/` path used by the control-plane Helmfile.
Prepare environment and secrets files separately from the packaged sources.

With a supporting CLI, `--umbrella-release X.Y.Z` selects the complete source
bundle. A local archive uses `--umbrella-bundle` plus `--umbrella-sha256`.
Do not combine these with explicit component source flags. Existing component
installation and per-stack upgrade procedures still apply; the umbrella version
does not make a multi-plane installation atomic.
