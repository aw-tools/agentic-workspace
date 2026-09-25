# Getting started

## Before you start

`aw` relies on three tools on your machine:

- git.
- garden, version 2.6 or later. `aw bootstrap` hands the cloning of members to
  it. Install it from https://github.com/garden-rs/garden.
- dprint, the formatter the pre-commit hook runs. Install it from
  https://dprint.dev/install/.

[Homebrew](https://brew.sh) installs both:

```sh
brew install garden dprint
```

`aw doctor` checks git and garden and says what is missing. It does not check
dprint, so install that yourself. Workspace scripts also use `sh` and `awk`,
which macOS and Linux provide.

## Install the tool

With Homebrew:

```sh
brew install aw-tools/tap/aw-cli
```

The formula is named for the project; the command is `aw`. `brew upgrade` moves
you to a newer release.

Without Homebrew, every release at
https://github.com/aw-tools/aw-cli/releases/latest carries four archives. Pick
the one that matches your machine:

| Your machine                        | Archive                               |
| ----------------------------------- | ------------------------------------- |
| Mac with Apple silicon              | `aw-aarch64-apple-darwin.tar.gz`      |
| Mac with an Intel processor         | `aw-x86_64-apple-darwin.tar.gz`       |
| Linux, most distributions           | `aw-x86_64-unknown-linux-gnu.tar.gz`  |
| Linux built on musl, such as Alpine | `aw-x86_64-unknown-linux-musl.tar.gz` |

Download it together with the checksums file, check it and put the binary on
your path. Set `target` to the middle part of your archive name:

```sh
target=x86_64-unknown-linux-gnu
base=https://github.com/aw-tools/aw-cli/releases/latest/download
curl -LO "$base/aw-$target.tar.gz" -O "$base/SHA256SUMS"
sha256sum -c SHA256SUMS --ignore-missing
tar xzf "aw-$target.tar.gz"
mkdir -p ~/.local/bin
install -m 755 aw ~/.local/bin/aw
```

On macOS the checksum command is `shasum -a 256 -c SHA256SUMS --ignore-missing`.
The rest is the same.

A browser download marks the archive as quarantined on macOS, and the binary
then refuses to start. Clear the mark with `xattr -d com.apple.quarantine aw`.
The `curl` command above leaves no mark.

Building from source needs a Rust toolchain at 1.85 or later. Install it with
rustup from https://rustup.rs, which gives you `cargo`:

```sh
cargo install --git https://github.com/aw-tools/aw-cli aw-cli
```

Check that the binary is on your path:

```sh
aw --version
```

A "command not found" means the directory holding `aw` is missing from your
`PATH`. Add `export PATH="$HOME/.local/bin:$PATH"` to your shell profile and
open a new terminal.

## Create the workspace

Pick a name. By convention the directory ends in `.workspace`, and the name
recorded in the manifest is the directory name without that suffix.

```sh
aw init myproject.workspace
cd myproject.workspace
```

`aw init` clones the template at its latest stable release, checks that it has a
deny-all `.gitignore` and a manifest, and runs `git init`. The manifest records
which release the workspace came from. It makes no commit. The first commit is
yours, so that you see what the workspace tracks before anything is recorded.

## Declare the members

Open `workspace.toml` and add one block per member:

```toml
[[repo]]
path = "service-api"
url = "git@github.com:example/service-api.git"
branch = "main"
```

`path` is where the member is checked out, relative to the workspace root.
`branch` is optional; without it the remote's default branch is used.

## Bootstrap

```sh
aw bootstrap
```

`aw bootstrap` clones each member that is missing, applies the manifest's
configuration and links any skills the members offer. It reports one line per
member.

Then look before you commit:

```sh
git status --porcelain
git ls-files
```

`git ls-files` lists every file the workspace tracks. If a member's file
appears, the `.gitignore` is wrong and you stop here. When the list is only the
layer, make the first commit.

## Choose a remote

A remote is optional. Without one the record lives on one machine and is durable
only there. Add one if you want the record on more than one machine, or want a
copy that survives the machine.

Choose the host for the record, not for the code. The code repositories carry
what a reader of the code may see. The record carries the whole thread of your
work: client names, machine facts, people, and decisions that were rejected.
Pick a host whose confidentiality you would accept for that whole record.

A hosted private repository suits personal or public work. A workspace that
names clients, infrastructure, or people belongs on infrastructure you control.

Decide before the first push. Switching later is one remote change, but the
history already pushed stays on the old host.

## Turn on the hook

Turn on the pre-commit hook:

```sh
bin/install-hooks
```

After that, dprint checks each commit's formatting and the lint checks the
record against its rules. A commit that fails either check is refused.

## Check the environment

```sh
aw doctor
```

`aw doctor` checks git and garden, whether each member's remote is reachable and
whether the skill links are healthy. Each finding comes with a remedy.

## The first session

Open the workspace in your agent. The agent reads `AGENTS.md`, which sends it to
the state file. The state file holds only its skeleton at this point. The first
session's job is to fill it: what is live, what comes next, what is blocked.

Before it reads anything, a session refreshes:

```sh
aw sync
aw status
```

`aw sync` fetches the workspace and every member. `aw status` reports each
repository's position against its remote and whether its working tree is clean.
A session never asserts a repository's state from memory: it fetches, then
reads.

`aw fast-forward` brings members that fell behind up to date. It fetches, then
moves only the members safe to move and reports the rest.
`aw fast-forward --help` lists what makes a member safe to move.

At the end of the session, the agent rewrites the state file to say where things
stand, commits the layer and pushes it. The next session, on any machine, starts
from that commit.

[Chapter 4](04-the-record.md) explains the record in full: what goes in each
file and why.
