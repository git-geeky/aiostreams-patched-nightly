# AIOStreams patched-nightly builder

This repository is the separate, immutable builder for the temporary AIOStreams
reader-health patch used while upstream PR #1206 is pending. It does not contain
application credentials and it never deploys an image. A workflow run consumes
an explicitly supplied runtime base SHA, reviewed upstream PR base and head,
official image digest, base-specific patch transplant, and reviewed overlay
commit. The workflow verifies the original PR delta independently, then applies
only the exact reviewed transplant whose parent is the frozen runtime base. This
makes a conflict resolution explicit instead of hiding it in a mutable CI
workspace. The overlay must be the transplant's direct child and contains only
sanitized reader-health telemetry and the narrow Premiumize account-denial
failover guard. The workflow rejects changes outside the approved inventories,
builds only `linux/amd64`, publishes an immutable GHCR image, and emits a
versioned provenance receipt plus registry attestations.

The production controller accepts only a digest and a matching provenance
tuple. The `nightly` and `stable` lanes are built separately from their frozen
source bases so a stable build never silently imports unrelated nightly commits.

`build-tamtaro-schema19.yml` is a separate, bounded compatibility workflow. It
builds an exact public source commit that backports Tamtaro Complete 3.2.6's
`folderSize()` and health-check requirements onto the reviewed AIOStreams 2.33.2
schema-19 line. It verifies a direct commit chain, exact source tree, unchanged
migration directory, maximum migration 0019, formatting, tests, and the full
monorepo build before publishing. Its lane-specific rollback digest and approval
artifact digest are embedded in both image labels and the attested provenance.
