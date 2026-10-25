# Third-party notices

This repository's first release contains documentation, benchmark task files and a small check script. It bundles no third-party code.

## Upstream projects in the Cupid CLI and VS Code extension

| Project | License | Where the notice is kept |
|---|---|---|
| opencode | MIT | [cli/LICENSE](cli/LICENSE), [vscode/LICENSE](vscode/LICENSE) |
| Kilo Code | MIT | [cli/LICENSE](cli/LICENSE), [vscode/LICENSE](vscode/LICENSE) |
| Continue | Apache-2.0 | [vscode/THIRD_PARTY_LICENSES/continue.txt](vscode/THIRD_PARTY_LICENSES/continue.txt) |

## Dependencies bundled in the binaries

License texts for components shipped with the Cupid CLI binary (for example the Linux sandbox helper `bwrap` from bubblewrap) are installed in the `licenses/` folder next to the binary (`~/.cupid/bin/licenses/`). A full generated list of the dependencies compiled into the binary will be added to this file when the CLI source is published.
