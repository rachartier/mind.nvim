# Fork

The [original mind.nvim](https://github.com/phaazon/mind.nvim) has been deprecated, so I wanted to pick up this project.

## Update Log

- Added option to specify where the popup should appear in a split
- Updated Nerd Font Icons that had been removed
- Removed deprecated code

<h1 align="center">mind.nvim</h1>

<p align="center">
  <img src="https://user-images.githubusercontent.com/506592/185793543-e12baf93-8329-4e3b-96d2-da38547691ee.png"/>
</p>

<p align="center">
  <img src="https://img.shields.io/github/issues/Selyss/mind.nvim?color=cyan&style=for-the-badge"/>
  <img src="https://img.shields.io/github/issues-pr/Selyss/mind.nvim?color=green&style=for-the-badge"/>
  <img src="https://img.shields.io/github/last-commit/Selyss/mind.nvim?style=for-the-badge"/>
  <img src="https://img.shields.io/github/v/tag/Selyss/mind.nvim?color=pink&label=release&style=for-the-badge"/>
</p>

<p align="center">
  <a href="#installation">Install</a>
</p>

This plugin is a new take on note taking and task workflows. The idea is derived from using several famous plugins, such
as [org-mode] or even standalone applications, like [Notion], and add new and interesting ideas.

<!-- vim-markdown-toc GFM -->

* [Motivation](#motivation)
* [Features](#features)
* [Getting started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Installation](#installation)
  * [Configuration](#configuration)
    * [Important note about versioning](#important-note-about-versioning)
    * [Nightly users](#nightly-users)
* [Usage](#usage)
* [Keybindings](#keybindings)

<!-- vim-markdown-toc -->

# Motivation

**Mind** is an organizer tool for Neovim. It can be used to accomplish and implement a wide variety of workflows. It is designed to quickly add items in trees. _Why a tree?_ Well, list of things like TODO lists are great but they lack the organization part. Most of them can be gathered in “lists of lists” — you probably have that on your phone. A list of
list is basically a tree. But editing and operating a list of list is annoying, so it’s better to have a tool that has
the concept of a node and a tree as a primitive.

**Mind** trees can be used to implement workflows like:

- Journaling. Have a node for each day, which parent will be the month, which parent will be the year, etc.
- Note taking. You are in the middle of a meeting and you heard something important? Don’t write that in a Markdown
  document in your `~/documents` that is probably already a mess: open your **Mind** tree and add it there!
- “Personal wiki.” Because of the nature of a tree, it is convenient to organize your personal notes about your work
  services, other teams’ products, OKRs, blablabla by simply creating trees in trees!
- Task management. Why not having a tasks tree with three or four sub-trees for your backlog, on-going work, finished
  work and cancelled tasks? It’s all possible!

The possibilities are endless.

# Features

**Mind** features two main concepts; global trees and local trees:

- A global tree is a tree that is unique to your machine / computer. Opening your main **Mind** tree from Neovim will
  always open and edit that tree. It’s basically your central place for your **Mind** nodes.
- A local tree is a tree that is relative to a given directory. **Mind** implements a `cwd`-based local form of tree, so
  you can even share those trees with other people (as long as they use **Mind** as well).

Atop of that, **Mind** has the concept of “project” trees, which are either a global tree, or a local tree. A global
project tree is stored at the same place as your main tree and the purpose of such a tree is to be opened only when your
`cwd` is the same as the tree, but you don’t want the tree to be in the actual `cwd`. That can be the case if you work
on a project where you don’t want to check the tree in Git or any versioning system.

On the other side, a local project tree is what it means: it lives in the `cwd`, under `.mind`, basically.

Besides that, **Mind** allows you to manipulate trees and nodes. Feature set:

- Everything is interactive and relies on the most recent features of Neovim, including `vim.ui.input` and
  `vim.ui.select`. Very few dependencies on other plugins, so you can customize the UI by using the plugins you love.
- Cursor-base interaction. Open a tree and start interacting with it!
  - Expand / collapse nodes.
  - Add a node to a tree by adding it before or after the current node, or by adding it inside the current node at the
    beginning or end of its chilren.
  - Rename the node under the cursor.
  - Change the icon of the node under the cursor.
  - Delete the node under the cursor with a confirmation input.
  - Select a node to perform further operations on it.
  - Move nodes around!
  - Select nodes by path! — e.g. `/Tasks/On-going/3345: do this`
- Supports user keybindings via keymaps. Keymaps are namespaced keybindings. They keymaps are fixed and defined by
  **Mind**, and users can decide what to put in them. For instance, you have the _default_ keymap for default
  navigation, _selection_ keymap for when a node is selected, etc. etc.
- Nodes are just text, icons and some metadata by default. You can however decide to associate them with a _data file_,
  for which the type is user-defined (by default Markdown), or you can turn them into URL nodes.
- A data node will open its file when triggered.
- A URL node will open its link when triggered.
- A well documented Lua API to create your own automatic workflow that don’t require user interaction!
- More to come!

# Getting started

This section will guide you through the list of steps you must take to be able to get started with **Mind**.

## Prerequisites

This plugin was written against Neovim 0.7.2, so you need to ensure you are running **Neovim 0.7.2 or higher**.

Lua dependencies:

- [plenary.nvim](https://github.com/nvim-lua/plenary.nvim), which can be required with `'nvim-lua/plenary.nvim'`.
- [nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons) (used for icons) which can be required with `'nvim-tree/nvim-web-devicons'`

## Installation

*It is strongly discouraged to use `master` as that branch can introduce breaking changes at any time.*
Refer to [versioning](https://github.com/Selyss/mind.nvim/tree/master#important-note-about-versioning) for more information

### [lazy.nvim](https://github.com/folke/lazy.nvim)
```lua
{
  "Selyss/mind.nvim",
  branch = 'v2.2',
  dependencies = { 
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- optional, used for icons
  },
  opts = {
    -- your configuration comes here
  }
}
```

### [packer.nvim](https://github.com/wbthomason/packer.nvim)
```lua
use {
  'Selyss/mind.nvim',
  branch = 'v2.2',
  requires = { 
    "nvim-lua/plenary.nvim",
    "nvim-tree/nvim-web-devicons", -- optional, used for icons
  },
  config = function()
    require'mind'.setup()
  end
}
```

This will bring you a default experience. Feel free to customize later the `setup` invocation (`:h mind.setup`).

# Configuration
The following are the default options. Refer to the installation guide for the location of options.

```lua
{
  -- persistence, both for the tree state and data files
  persistence = {
    -- path where the global mind tree is stored
    state_path = '~/.local/share/mind.nvim/mind.json',

    -- directory where to create global data files
    data_dir = '~/.local/share/mind.nvim/data',
  },

  -- edit options
  edit = {
    -- file extension to use when creating a data file
    data_extension = '.md',

    -- default header to put in newly created data files
    data_header = '# %s',

    -- format string for copied links
    copy_link_format = '[](%s)',
  },

  -- tree options
  tree = {
    -- automatically create nodes (when looking for paths for example)
    automatic_creation = true,

    -- automatically create data file when trying to open one that doesnΓÇÖt
    -- have any data yet
    automatic_data_creation = false,
  },

  -- UI options
  ui = {
    -- split to open window in
    position = 'left',

    -- commands used to open URLs
    url_open = url_open,

    -- default width of the tree view window
    width = 30,

    -- should mind window be closed when a file is opened
    close_on_file_open = false,

    -- marker used for empty indentation
    empty_indent_marker = 'Γöé',

    -- marker used for node indentation
    node_indent_marker = 'Γöö',

    -- marker used to identify the root of the tree (left to its name)
    root_marker = '∩Ç¡ ',

    -- marker used to identify a local root (right to its name)
    local_marker = 'local',

    -- marker used to show that a node has an associated data file
    data_marker = '≤░êÖ ',

    -- marker used to show that a node has an URL
    url_marker = '∩æî ',

    -- marker used to show that a node is currently selected
    select_marker = '∩ää',

    -- highlight options
    highlight = {
      -- highlight used on closed marks
      closed_marker = 'LineNr',

      -- highlight used on open marks
      open_marker = 'LineNr',

      -- highlight used on the name of the root node
      node_root = 'Function',

      -- highlight used on regular nodes with no children
      node_leaf = 'String',

      -- highlight used on regular nodes with children
      node_parent = 'Title',

      -- highlight used on the local marker
      local_marker = 'Comment',

      -- highlight used on the data marker
      data_marker = 'Comment',

      -- highlight used on the url marker
      url_marker = 'Comment',

      -- highlight used on empty nodes (i.e. no children and no data)
      modifier_empty = 'Comment',

      -- highlight used on the selection marker
      select_marker = 'Error',
    },

    -- preset of icons
    icon_preset = {
      { '∩Ç¡ ', 'Sub-project' },
      { '≤░Äò ', 'Journal, newspaper, weekly and daily news' },
      { '≤░¢¿ ', 'For when you have an idea' },
      { '∩üä ', 'Note taking?' },
      { '≤░ùç', 'Task management' },
      { '∩éû ', 'Uncheck, empty square or backlog' },
      { '∩âê ', 'Full square or on-going' },
      { '∩üå ', 'Check or done' },
      { '∩ç╕ ', 'Trash bin, deleted, cancelled, etc.' },
      { 'ε£ë ', 'GitHub' },
      { '≤░ì║ ', 'Monitoring' },
      { '≤░çº ', 'Internet, Earth, everyone!' },
      { '∩ï£ ', 'Frozen, on-hold' },
    },
  },

  -- default keymaps; see 'mind.commands' for a list of commands that can be mapped to keys here
  keymaps = {
    -- keybindings when navigating the tree normally
    normal = {
      ['<cr>'] = 'open_data',
      ['<s-cr>'] = 'open_data_index',
      ['<tab>'] = 'toggle_node',
      ['<s-tab>'] = 'toggle_parent',
      ['/'] = 'select_path',
      ['$'] = 'change_icon_menu',
      c = 'add_inside_end_index',
      I = 'add_inside_start',
      i = 'add_inside_end',
      l = 'copy_node_link',
      L = 'copy_node_link_index',
      d = 'delete',
      D = 'delete_file',
      O = 'add_above',
      o = 'add_below',
      q = 'quit',
      r = 'rename',
      R = 'change_icon',
      u = 'make_url',
      x = 'select',
    },

    -- keybindings when a node is selected
    selection = {
      ['<cr>'] = 'open_data',
      ['<tab>'] = 'toggle_node',
      ['<s-tab>'] = 'toggle_parent',
      ['/'] = 'select_path',
      I = 'move_inside_start',
      i = 'move_inside_end',
      O = 'move_above',
      o = 'move_below',
      q = 'quit',
      x = 'select',
    },
  },
}
```


# Usage

| Commands                            | Description                                                                                                                                                 |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:MindOpenMain`                     | Open the main Mind tree.
| `:MindOpenProject`                  | Open the project tree.
| `:MindOpenSmartProject`             | Open the project tree, either local, global or prompt the user for which kind of project tree to create.
| `:MindReloadState`                  | Reload Mind state for global and local trees.
| `:MindClose`                        | Close project or main Mind tree if open. Resets ui cache.

*A wiki is planned, but for now, you can simply have a look at `:h mind-usage` and `:h mind-commands`.*

### Important note about versioning

This plugin implements [SemVer] via git branches and tags. Versions are prefixed with a `v`, and only patch versions
are git tags. Major and minor versions are git branches. You are **very strongly advised** to use a major version
dependency to be sure your config will not break when Mind gets updated.

- Major versions always have the form `vM`, where `M` is the major version. — e.g. `v2`.
- Minor versions always have the form `vM.N`, where `M` is the major version and `N` the minor. — e.g. `v2.0`.
- Patch versions always have the form `vM.N.P`, where `M` is the major version, `N` the minor and `P` the patch. — e.g.
  `v2.0.0`.

**It is strongly discouraged to use `master` as that branch can introduce breaking changes at any time.**

### Nightly users

Mind supports nightly releases of Neovim. However, keep in mind that if you are on a nightly version, you must be **on
the last one**. If you are not, then you are exposed to Neovim compatibility issues / breakage.

# Keybindings

The user commands defined by Mind are mapped to no keybindings by default. However, once you have a tree open,
buffer-local keybindings are automatically inserted. You can change them by setting they behaviour you want in
`opts.keymaps`. More information about that in `:h mind-config-keymaps`.

### Normal Mode

| Mappings       | Action                                               |
|----------------|------------------------------------------------------|
| `<cr>`         | open_data                                            |
| `<s-cr>`       | open_data_index                                      |
| `<tab>`        | toggle_node                                          |
| `<s-tab>`      | toggle_node                                          |
| `/`            | select_path                                          |
| `$`            | change_icon_menu                                     |
| `c`            | add_inside_end_index                                 |
| `I`            | add_inside_start                                     |
| `i`            | add_inside_end                                       |
| `l`            | copy_node_link                                       |
| `L`            | copy_node_link_index                                 |
| `d`            | delete                                               |
| `D`            | delete_file                                          |
| `O`            | add_above                                            |
| `o`            | add_below                                            |
| `q`            | quit                                                 |
| `r`            | rename                                               |
| `R`            | change_icon                                          |
| `u`            | make_url                                             |
| `x`            | select                                               |

### Selection Mode

| Mappings       | Action                                               |
|----------------|------------------------------------------------------|
| `<cr>`         | open_data                                            |
| `<s-tab>`      | toggle_node                                          |
| `/`            | select_path                                          |
| `I`            | move_inside_start                                    |
| `i`            | move_inside_end                                      |
| `O`            | move_above                                           |
| `o`            | move_below                                           |
| `q`            | quit                                                 |
| `x`            | select                                               |

[org-mode]: https://orgmode.org/
[Notion]: https://www.notion.so/
[SemVer]: https://semver.org
