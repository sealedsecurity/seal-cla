# seal-cla

Contributor License Agreement signatures for [`sealedsecurity/seal`](https://github.com/sealedsecurity/seal).

## What lives here

`signatures/version1/cla.json` — append-only JSON log of every contributor who has agreed to the [Sealed Security CLA](https://github.com/sealedsecurity/seal/blob/main/CLA.md). Each entry records:

- The signer's GitHub login + numeric ID.
- The PR number where they signed.
- The timestamp of the signing comment.

The file is managed exclusively by the [CLA Assistant workflow](https://github.com/sealedsecurity/seal/blob/main/.github/workflows/cla.yml) on `sealedsecurity/seal`. **Don't edit it by hand** — every commit here should be authored by the CLA Assistant bot via the action's `PERSONAL_ACCESS_TOKEN`-authenticated path.

## How a signature gets here

1. A contributor opens a PR on `sealedsecurity/seal`.
2. The `CLA Assistant` workflow checks their GitHub login against `signatures/version1/cla.json`. If missing, the workflow posts a "please sign" comment on the PR with an exact sign phrase.
3. The contributor comments the sign phrase on the PR.
4. The workflow re-runs, appends a `signedContributors` entry to this repo's JSON file via `octokit.repos.createOrUpdateFileContents`, and flips the PR's CLA check to pass.

The CLA Assistant workflow uses `CLA_PAT` (an organization-level secret pointing at a fine-grained PAT scoped to this repo's Contents R/W) to authenticate the write. The PAT must remain valid; expired PAT → CLA Assistant fails on every new contributor.

## Recovery

If the signatures file is corrupted, restore the previous good state via `git push --force` from a local clone. The action is idempotent — once the file is restored, re-running the workflow on any open PR re-verifies signatures cleanly.

## License

The CLA text contributors are signing is [CLA.md in the seal repo](https://github.com/sealedsecurity/seal/blob/main/CLA.md). The signatures here are released under Apache-2.0, matching the seal/ permissive-carve-out crates' license.
