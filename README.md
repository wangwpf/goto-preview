## ⭐ Goto Preview
A small Neovim plugin for previewing native LSP's goto definition calls in floating windows.

### 🚀 Showcase
<img src="https://github.com/rmagatti/readme-assets/blob/main/goto-preview-zoomed.gif" />

#### 🔗 References
<img src="https://github.com/rmagatti/readme-assets/blob/main/goto-preview-references.gif" />

### ⚠️ IMPORTANT NOTE
Make sure you use Neovim `0.5.1` or GUIs like [Goneovim](https://github.com/akiyosi/goneovim) and [Uivonim](https://github.com/smolck/uivonim).

There is a bug in [Neovim `0.5`](https://github.com/neovim/neovim/issues/14735) that prevents the correct positioning of more than one preview window.

### 📦 Installation
Packer.nvim
```lua
use {
  'rmagatti/goto-preview',
  config = function()
    require('goto-preview').setup {}
  end
}
```

### ⚙️ Configuration

**Default**
```lua
require('goto-preview').setup {
  width = 120; -- Width of the floating window
  height = 15; -- Height of the floating window
  border = {"↖", "─" ,"┐", "│", "┘", "─", "└", "│"}; -- Border characters of the floating window
  default_mappings = false; -- Bind default mappings
  debug = false; -- Print debug information
  opacity = nil; -- 0-100 opacity level of the floating window where 100 is fully transparent.
  resizing_mappings = false; -- Binds arrow keys to resizing the floating window.
  post_open_hook = nil; -- A function taking two arguments, a buffer and a window to be ran as a hook.
  references = { -- Configure the telescope UI for slowing the references cycling window.
    telescope = telescope.themes.get_dropdown({ hide_preview = false })
  };
  -- These two configs can also be passed down to the goto-preview definition and implementation calls for one off "peak" functionality.
  focus_on_open = true; -- Focus the floating window when opening it.
  dismiss_on_move = false; -- Dismiss the floating window when moving the cursor.
  force_close = true, -- passed into vim.api.nvim_win_close's second argument. See :h nvim_win_close
  bufhidden = "wipe", -- the bufhidden option to set on the floating window. See :h bufhidden
}
```

The `post_open_hook` function gets called right before setting the cursor position in the new floating window.
One can use this to set custom key bindings or really anything else they want to do when a new preview window opens.

### ⌨️ Mappings
There are no mappings by default, you can set `default_mappings = true` in the config to make use of the mappings I use or define your own.

**Default**
```viml
nnoremap gpd <cmd>lua require('goto-preview').goto_preview_definition()<CR>
nnoremap gpi <cmd>lua require('goto-preview').goto_preview_implementation()<CR>
nnoremap gP <cmd>lua require('goto-preview').close_all_win()<CR>
" Only set if you have telescope installed
nnoremap gpr <cmd>lua require('goto-preview').goto_preview_references()<CR>
```

**Custom example**
```lua
vim.api.nvim_set_keymap("n", "gp", "<cmd>lua require('goto-preview').goto_preview_definition()<CR>", {noremap=true})
```

### Window manipulation
One can manipulate floating windows with the regular Vim window moving commands. See `:h window-moving`.
Example:
<img src="https://user-images.githubusercontent.com/2881382/121652080-88716e00-ca58-11eb-811c-677ec61d8e25.gif" />

### Supported languages
Goto Preview should work with LSP responses for most languages now! If something doesn't work as expected, drop an issue and I'll be happy to check it out!

**Note:** different language servers have potentially different shapes for the result of the `textDocument/definition` and `textDocument/implementation` calls.
Until more are added one can pass in custom responses through the `lsp_configs` config value. Just follow the same pattern returning two values, a `target (string)` and a `cursor_position ({line_num, col_num})`. The `data` parameter is the `[1]` of the LSP's `result` of the definition/implementation calls and is what gets passed into the custom `get_config` function.


### Tested with
```
NVIM v0.5.0-dev+7d4f890aa
Build type: Release
LuaJIT 2.1.0-beta3
```
