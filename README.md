# AIOStreams patched-nightly builder

This repository is the separate, immutable builder for the temporary AIOStreams
reader-health patch used while upstream PR #1206 is pending. It does not contain
application credentials and it never deploys an image. A workflow run consumes
an explicitly supplied runtime base SHA, reviewed PR base SHA, official image
digest, PR head, and reviewed overlay commit. The patch is derived only from the
reviewed PR base-to-head delta, then applied to the lane's runtime base. The
overlay is likewise derived from the exact reviewed child commit rather than
trusting a mutable workspace patch. It contains only sanitized reader-health
telemetry and the narrow Premiumize account-denial failover guard. The workflow
rejects conflicts or changes outside the approved inventories, builds only
`linux/amd64`, publishes an immutable GHCR image, and emits a provenance receipt
plus registry attestations.

The production controller accepts only a digest and a matching provenance
tuple. The `nightly` and `stable` lanes are built separately from their frozen
source bases so a stable build never silently imports unrelated nightly commits.
