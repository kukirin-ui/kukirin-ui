Hi, I'm kukirin.

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
**My fix:** [docs/pos-range-guidance](https://github.com/kukirin-ui/neovim/commit/85811a9)

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

Find me on X as [@termuxSolutions](https://x.com/termuxSolutions) or on [XDA](https://xdaforums.com/m/termuxsolutions.13482532/).
