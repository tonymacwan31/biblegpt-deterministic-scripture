# Verify the local artifact

The payload manifest is `public_artifact_manifest_v2.json` (not
`release_candidate_manifest.json`). The 118-file, 18,067,252-byte inventory
and identities below describe the extracted payload tree, excluding the
manifest from the aggregate inventory as specified by the v2 format.

## Check a downloaded transport archive

From the directory containing the archive, run:

```sh
shasum -a 256 BibleGPT-Deterministic-Scripture-1.0.0-macos-arm64-py3.14.6.tar.gz
```

Compare the result with the archive-specific entry in `SHA256SUMS`. This is a
transport checksum for the `.tar.gz` file. It is not the extracted payload's
whole-tree identity.

## Check the extracted payload

From the extracted `BibleGPT-Deterministic-Scripture-1.0.0/` directory:

```sh
python3 scripts/verify_public_artifact.py .
shasum -a 256 public_artifact_manifest_v2.json
```

The included verifier checks **structural artifact integrity**: it validates
the v2 artifact structure and manifest. The immutable artifact's historical
approval is a separate check. Its recorded publication scope is false, so the
verifier may correctly report `STRUCTURALLY SAFE` followed by
`PUBLIC RELEASE NOT APPROVED` (and a nonzero exit). That result does not
invalidate structural integrity or change the historical approval.

The later, additive `PUBLICATION_AUTHORIZATION.json` is a third check. It
binds the exact artifact identities below to the authorized repository
`tonymacwan31/biblegpt-deterministic-scripture`, tag `v1.0.0`, archive, and
publication scope. It is published in the metadata repository and attached
unchanged to the GitHub Release; it is not inside the immutable 118-file
artifact. Verify that its closed schema, canonical record identity,
destination, authorization scope, and artifact bindings match this release.
Do not interpret structural integrity, historical artifact approval, or the
additive publication authorization as interchangeable checks.

The expected manifest SHA-256 is
`1707049624326eb126aead29689b27a2b254945fa05328dbb06041d5ad6d7794`.
The expected deterministic manifest identity is
`a06ea67dc5495f9d74c34e1bf5ed015855c964489d61a2e13db69a79d629bd48`; the
aggregate identity is
`ac7bb6ba833bb49ef76399a2cc34bb26d658044ed063ff8308ee30b0ce59c796`; and the
whole-tree identity is
`sha256-framed-file-tree-v1:0e414aff0901cd63c1678dc8dd13506d280afdcac8e1cc9ac75b920675c299cd`.

Structural verification is not publication authorization. The historical
artifact-specific owner approval is not a publication approval: publication,
distribution, deployment, and production migration remain unauthorized.
