tags: #dotconfig
Ipython can be configured as well - [here are the docs](https://ipython.org/ipython-doc/3/config/intro.html)

I mainly make use of this to change the prompt depending of if I am in insert mode or not (my terminal emulators usually operate with some kind of vim mode activated)

Start a config by running
```bash
ipython profile create
```

Then under `~/.ipython/profile_default/ipython_config.py` activate this

```python

## Shortcut style to use at the prompt. 'vi' or 'emacs'.
#  Default: 'emacs'
c.TerminalInteractiveShell.editing_mode = 'vi'

...


## Cursor shape changes depending on vi mode: beam in vi insert mode, block in
#  nav mode, underscore in replace mode.
#  Default: True
c.TerminalInteractiveShell.modal_cursor = True
```