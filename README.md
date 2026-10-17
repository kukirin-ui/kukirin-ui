Hi, I'm Alen.

I mostly mess around with Termux, Android and Linux. On XDA I go by termuxSolutions and answer questions about adb, fastboot, flashing, recoveries and getting Linux onto phones and tablets.

Stuff I made:
- [termuxSolutions](https://github.com/kukirin-ui/termuxSolutions): my XDA answers in one place
- [Meridian](https://github.com/kukirin-ui/Meridian): an Android command deck for Termux
- [Nullcloud](https://github.com/kukirin-ui/Nullcloud): run llama.cpp on your phone and chat from any browser, offline

Recent contributions:
- termux/termux-packages: [#32600](https://github.com/termux/termux-packages/pull/32600)
- tldr-pages/tldr: [#24723](https://github.com/tldr-pages/tldr/pull/24723)
- voidcosmos/npkill: [#275](https://github.com/voidcosmos/npkill/pull/275)
- LargeModGames/spotatui: [#860](https://github.com/LargeModGames/spotatui/pull/860), [#861](https://github.com/LargeModGames/spotatui/pull/861)

## Spotlight: neovim `vim.pos` / `vim.range` docs

<table><tr><td>

**Repo:** [neovim/neovim](https://github.com/neovim/neovim) (103k ⭐)  
**Issue:** [#39868 docs: guidance for vim.pos, vim.range](https://github.com/neovim/neovim/issues/39868)  
**My fix:** [docs/pos-range-guidance](https://github.com/kukirin-ui/neovim/commit/85811a9dab18c61fa810d71c244790f99a43bf2a)

</td></tr></table>

I picked this one up on my own, without asking first. From `K` you couldn't tell whether a constructor wanted (1,0) or (0,0), so I put the indexing in every summary line:

| Constructor | Indexing |
|---|---|
| `cursor`, `mark` | (1,0) |
| `extmark` | (0,0) |
| `lsp` | 0-based line, offset in `position_encoding` |
| `offset` | 0-based byte offset |

I also fixed `vim.range.to_mark()`, which said it returned an extmark range. It returns a mark range.

✅ `make lintdoc` passed  
✅ `pos_spec` 7/7 and `range_spec` 15/15 passed  
✅ Only doc comments changed, no code

The PR was closed without merging, but the fix is still there if anyone wants it.

## Spotlight: Zig on old Android kernels (Termux)

<table><tr><td>

**Repo:** [termux/termux-packages](https://github.com/termux/termux-packages) (17k ⭐)  
**Issue:** [#20783 Running some zig commands gives "illegal instruction"](https://github.com/termux/termux-packages/issues/20783)  
**My fix:** [#32600](https://github.com/termux/termux-packages/pull/32600)

</td></tr></table>

On aarch64, Zig reads CPU ID registers with raw `mrs`. Kernels older than 4.11 don't emulate that for userspace, so some zig commands (like `zig run`) died with "Illegal instruction". I made it check `HWCAP_CPUID` first and fall back to the baseline CPU when it's missing.

✅ Patch applies to Zig 0.16.0, `zig ast-check` passed  
✅ Cross-compiled aarch64 test still detects `cortex_a53` under qemu  
✅ ncdu2 and zls build fine with the patched Zig  
✅ On a tester's real 4.9 device the "Illegal instruction" crash is gone

The same device then stopped on `statx`, a syscall that only exists from kernel 4.11. Zig dropped support for those kernels on purpose, and a maintainer preferred a full fix or none, so I closed the PR rather than carry a half fix.

## Spotlight: Bootstrap card docs

<table><tr><td>

**Repo:** [twbs/bootstrap](https://github.com/twbs/bootstrap) (175k ⭐)  
**Issue:** [#43094 Style rules for `.card-title` are missing](https://github.com/twbs/bootstrap/issues/43094)  
**My fix:** [#43100](https://github.com/twbs/bootstrap/pull/43100)

</td></tr></table>

In v6 `.card-title` and `.card-text` stopped adding styles on purpose, but the card page didn't say so, so it looked like CSS was missing. I tracked down the commit that removed them and updated the docs to say both classes are hooks only.

✅ `docs-mdx` and `docs-md` lint passed  
✅ Prettier check passed  
✅ `css-lint` passed  
✅ Docs only, no CSS or JS changed

A maintainer went with a shorter one-sentence version in [#43103](https://github.com/twbs/bootstrap/pull/43103), merged the same day, so mine was closed.

## Spotlight: OpenClaw gateway under Termux proot

<table><tr><td>

**Repo:** [openclaw/openclaw](https://github.com/openclaw/openclaw) (392k ⭐)  
**Issue:** [#171385 GatewayLockNamespaceError in Termux proot Ubuntu](https://github.com/openclaw/openclaw/issues/171385)  
**My comment:** [diagnosis](https://github.com/openclaw/openclaw/issues/171385#issuecomment-6158241377)

</td></tr></table>

From 2026.10.x the gateway refused to start inside proot-distro Ubuntu on Termux. I traced it to the new PID namespace check failing under proot, suggested pinning the last working version as a stopgap, and detecting proot as the long-term fix.

✅ The failure was in the PID namespace probe under proot  
✅ Fixed upstream in [#174974](https://github.com/openclaw/openclaw/pull/174974)

The maintainer found the real mechanism: proot fakes `/proc/self/ns/pid`, so `stat()` on it fails while `readlink()` works. The fix reads the namespace from `readlink()`.

Find me on X as [@termuxSolutions](https://x.com/termuxSolutions) or on [XDA](https://xdaforums.com/m/termuxsolutions.13482532/).
