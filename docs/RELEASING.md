# Releasing

1. Run `bash tests/render-test.bash` and commit.
2. Tag and push: `git tag vX.Y.Z && git push origin main vX.Y.Z`. The `Release` workflow tests and creates
   the GitHub release. The formula builds from the source archive of the tag.
3. Update the formula in [`g4rcez/homebrew-tap`](https://github.com/g4rcez/homebrew-tap): set `url` to the new
   tag and `sha256` to the checksum of the archive:

   ```sh
   curl -sL https://github.com/g4rcez/rexpanes/archive/refs/tags/vX.Y.Z.tar.gz | shasum -a 256
   ```
