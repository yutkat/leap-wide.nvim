# leap-wide.nvim

[leap.nvim] has a problem on labelling multi-width characters.

This plugin provides an ad-hoc way to fix the issue.

The problem should actually be solved by [leap.nvim] itself or by `vim.api.nvim_buf_set_extmark`.

## Usage

``` lua
---@param ... any arguments passed to leap.leap()
local function leap(...)
  -- call fix_label() on LeapEnter to overlay spaces with the length same as the strdisplaywidth of base text to be labelled
  local autocmd = vim.api.nvim_create_autocmd("User", {
    pattern = "LeapEnter",
    once = true,
    callback = function()
      -- if you want to update leap.state, do it before fix_label()
      require("leap-wide").fix_label()
    end
  })
  local ok, err = pcall(require("leap").leap, ...)
  if not ok then
    -- ensure LeapEnter autocmd is deleted when leap fails without invoking the event
    pcall(vim.api.nvim_del_autocmd, autocmd)
    error(err)
  end
end
```

[leap.nvim]: https://github.com/ggandor/leap.nvim
