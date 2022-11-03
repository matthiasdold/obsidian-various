### For opening iron nvim in a vsplit with a given size

The problem with the new version is that the window is spawned as float:

```lua
--- Creates a new window for placing a repl.
-- Expected to be called before creating the repl.
-- It knows nothing about the repl and only takes in account the
-- configuration.
-- @warning might change the current window
-- @param bufnr buffer to be used
-- @return window id of the newly created window
ll.new_window = function(bufnr)
  if type(config.repl_open_cmd) == "function" then
    local result = config.repl_open_cmd(bufnr)
    if type(result) == "table" then
      return view.openfloat(result, bufnr)
    else
      return result
    end
  else
    vim.cmd(config.repl_open_cmd)
    vim.api.nvim_set_current_buf(bufnr)
    return vim.fn.bufwinid(bufnr)
  end
end
```

In my current setup, the `config.repl_open_cmd` indeed returned a lua function. 

--> My fix is just to use a non-function type command which will be directly used in vim.cmd() then.

See my issue raised with [iron.nvim](https://github.com/hkupty/iron.nvim/issues/267)