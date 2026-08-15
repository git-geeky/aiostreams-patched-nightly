# AIOStreams patched-nightly builder

This repository is the separate, immutable builder for the temporary AIOStreams
reader-health patch used while upstream PR #1206 is pending. It does not contain
application credentials and it never deploys an image. A workflow run consumes
an explicitly supplied upstream base SHA, official image digest, PR head, and
instrumentation commit; it rejects conflicts or changes outside the approved
patch inventory, builds only `linux/amd64`, publishes an immutable GHCR image,
and emits a provenance receipt plus a registry build attestation.

The production controller accepts only a digest and a matching provenance
tuple. The `nightly` and `stable` lanes are built separately from their frozen
source bases so a stable build never silently imports unrelated nightly commits.
