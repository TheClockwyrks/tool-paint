# history

The record of how this repository's history was extracted from the monorepo,
[The Test Cabinet](https://github.com/TheClockwyrks/TheTestCabinet), where the
crate here lived until the extraction. Nothing builds or tests against these
files; they are kept so the provenance outlives the scratch clone the
extraction ran in.

## The extraction

The history below the scaffold commit is the monorepo's `issue/repos-4` at
`77c83390e2c2c58e9df7e84ac44fff5d295af8d0`, the commit that moved the crate's
two model-core dependencies onto the tool-model-core repository, filtered to
the paths in [`paths.txt`](paths.txt) by git filter-repo 2.47.0:

```sh
git clone -q --no-local --single-branch --branch issue/repos-4 --no-tags \
  <the-test-cabinet> x
cd x
uvx --from git-filter-repo==2.47.0 git-filter-repo --force \
  --paths-from-file ../paths.txt --prune-empty always --preserve-commit-hashes
```

Of the 6,972 commits reachable from that commit, 9 touched a path in the list
and were kept (2 of them merges); the rest were pruned. No tag was carried. The
tip tree of `crates/paint/` hashes the same as the monorepo's at that commit
(`a8afc8bac4ccc0402329ce774f8b8ec015d6116d`).

The path is kept where it was in the monorepo, so the filter needed no rename.

## The files

- [`paths.txt`](paths.txt) is the list handed to `--paths-from-file`
  (sha256 `18eabe6bb1242fa3c257dcc75d632007cc897af8bd582ad8b51dfd051188c129`).
  It is generated, not hand-written: the moved directory (`crates/paint/`), and
  every path a tracked file under it had before a rename or copy, collected by
  `git log --follow -M -C --name-status` over each of the 42 files. No file
  under it was ever renamed or copied from elsewhere, so the list is that one
  directory.
- [`commit-map`](commit-map) is filter-repo's map from each monorepo commit (left)
  to the commit it became here (right), all zeros for a pruned one.
  `--preserve-commit-hashes` left the commit ids quoted in the messages as they
  were, so an id in a message below the scaffold commit names a monorepo commit,
  and this map translates it.
