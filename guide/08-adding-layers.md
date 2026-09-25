# Adding layers

You add each optional layer to a workspace you already have. The layers are
independent, so add only the ones you want, in any order.
[Chapter 7](07-going-further.md) says what each layer gives you.

## Skills

The public skills come from one member, `portable-skills`. Its skills sit in its
`skills` directory, so the manifest entry names that directory. The entry's
`agents` line links the kernel's agent definitions.

1. Add the member to `workspace.toml`:

   ```toml
   [[repo]]
   path = "portable-skills"
   url = "https://github.com/attila/portable-skills.git"
   skills = { dirs = ["skills"] }
   agents = { dirs = ["skills/orchestration-kernel/agents"] }
   ```

2. Clone it and link its skills and agent definitions:

   ```sh
   aw bootstrap
   ```

3. Check the links:

   ```sh
   aw doctor
   ```

`aw bootstrap` reports two skills and two agent definitions linked. `aw doctor`
reports no broken skill links.

To take specific skills only, add an `only` list naming them to the entry, such
as `only = ["aw-orienting"]`. Keep `orchestration-kernel` in it, because each
agent definition runs a script from that skill.

## The orchestration kernel

The orchestration kernel and its two agent definitions arrive with the skills
layer. The kernel's Claude Code scripts read JSON with `jq`, so on Claude Code
you install `jq` as well.

1. Add the skills layer above.
2. Install `jq`, with `brew install jq` or your package manager.

## Sandbox profiles and dotfiles

The example profiles live in the `nono` module of `portable-dotfiles`. You link
the module into your home directory with GNU Stow.

1. Install GNU Stow and nono, with `brew install stow nono` or your package
   manager.
2. Fork the repository on GitHub, then clone your fork into your home directory.
   The profiles grant access to that path. Your fork is a starting point, so cut
   it down to what you use.

   ```sh
   git clone https://github.com/<you>/portable-dotfiles.git ~/portable-dotfiles
   cd ~/portable-dotfiles
   ```

3. Link the module into your home directory:

   ```sh
   stow -t ~ nono
   ```

4. Put the Node wrapper the profiles use at the front of your `PATH`. Node tools
   then reach the network through the sandbox. Choose one:
   - run `stow -t ~ zsh`, which replaces your zsh configuration;
   - add `~/.local/share/nono-helpers/bin` to the front of `PATH` yourself, and
     keep it first after any tool that rewrites `PATH`.

   Stow refuses to replace a file that already exists, such as your own
   `~/.zshenv`. Move that file aside first.
5. Open a new shell, then check the Node wrapper:

   ```sh
   ~/portable-dotfiles/nono/.local/share/nono-helpers/smoke-test.sh
   ```

   It prints a line ending in `passed` when the wrapper works.
6. Install the nono package for each agent you run:

   ```sh
   nono pull nolabs-ai/claude
   nono pull nolabs-ai/codex
   ```

7. Adapt the profiles to your machine, following the checklist in the
   [nono section](https://github.com/attila/portable-dotfiles#nono-profiles) of
   the repository's `README.md`. Check the filesystem writes and credential
   access of the profile you choose.
8. Start the agent inside the profile, from the directory it works in:

   ```sh
   nono run --allow-cwd --profile claude-code-lore -- claude
   ```

`stow -D -t ~ nono` removes the links again.

## Lore

Lore serves your own pattern files to your agent. It needs Ollama running, and a
git repository of pattern files to index.

1. Install and start Ollama, with Homebrew or your package manager:

   ```sh
   brew install ollama
   brew services start ollama
   ```

2. Install `lore` with Homebrew:

   ```sh
   brew install attila/tap/lore
   ```

   Without Homebrew, install it from a release, following the install section of
   the [lore README](https://github.com/attila/lore#install).
3. Point lore at your patterns. Without any yet, create a git repository with
   one pattern file, following the pattern authoring guide chapter 7 links.

   ```sh
   lore init --repo ~/my-patterns
   ```

4. Check every component:

   ```sh
   lore status
   ```

5. Clone the lore repository, which holds the Claude Code plugin, and start
   Claude Code with it:

   ```sh
   git clone https://github.com/attila/lore.git
   claude --plugin-dir lore/integrations/claude-code/
   ```

`lore init` pulls the embedding model first. `lore status` then counts the
pattern files indexed, as sources. The
[lore documentation](https://github.com/attila/lore#documentation) covers
configuration, the plugin's hooks and how search ranks patterns.
