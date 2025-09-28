# Lazy Vim (neo-vim) Configurations

## 1. Make neo-tree always open on the current file path

First install neo-tree. Open nvim and run:
```bash
:Lazy install nvim-neo-tree/neo-tree.nvim
```
Restart nvim.

Then add the following configuration to your `init.lua` file.

Open init.lua config
```bash
vim ~/.config/nvim/init.lua
```

Add this:
```lua
vim.api.nvim_create_autocmd("VimEnter", {
  callback = function()
    vim.cmd("Neotree filesystem reveal_force_cwd")
  end,
})
```
Restart nvim.