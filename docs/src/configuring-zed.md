---
title: Configuring Zed - Settings and Preferences
description: Configure Zed with the Settings Editor, JSON files, and project-specific overrides. Covers all settings options.
---

# Configuring Zed

This guide explains how Zed's settings system works, including the Settings Editor, JSON configuration files, and project-specific settings.

For visual customization (themes, fonts, icons), see [Appearance](./appearance.md).

## Settings Editor

The **Settings Editor** ({#kb zed::OpenSettings}) is the primary way to configure Zed. It provides a searchable interface where you can browse available settings, see their current values, and make changes.

To open it:

- Press {#kb zed::OpenSettings}
- Or run `zed: open settings` from the command palette

As you type in the search box, matching settings appear with descriptions and controls to modify them. Changes save automatically to your settings file.

> **Note:** Not all settings are available in the Settings Editor yet. Some advanced options, like language formatters, require editing the JSON file directly.

## Settings Files

### User Settings

Your user settings apply globally across all projects. Open the file with {#kb zed::OpenSettingsFile} or run `zed: open settings file` from the command palette.

The file is located at:

- macOS: `~/.config/zed/settings.json`
- Linux: `~/.config/zed/settings.json` (or `$XDG_CONFIG_HOME/zed/settings.json`)
- Windows: `%APPDATA%\Zed\settings.json`

The syntax is JSON with support for `//` comments.

### Default Settings

To see all available settings with their default values, run {#action zed::OpenDefaultSettings} from the command palette. This opens a read-only reference you can use when editing your own settings.

### Project Settings

Override user settings for a specific project by creating a `.zed/settings.json` file in your project root. Run {#action zed::OpenProjectSettings} to create this file.

Similarly to user files, you can open your project settings file by running {#action zed::OpenProjectSettings} from the command palette.
This will create a `.zed` directory containing`.zed/settings.json`.

Project settings take precedence over user settings for that project only.

Although most projects will only need one settings file at the root, you can add more local settings files for subdirectories as needed.
Not all settings can be set in local files, just those that impact the behavior of the editor and language tooling.
For example you can set `tab_size`, `formatter` etc. but not `theme`, `vim_mode` and similar.

The syntax for configuration files is a super-set of JSON that allows `//` comments.

## Per-release Channel Overrides

Zed reads the same `settings.json` across all release channels (Stable, Preview or Nightly).
However, you can scope overrides to a specific channel by adding top-level `stable`, `preview`, `nightly` or `dev` objects.
They are merged into the base configuration with settings from these keys taking precedence upon launching the specified build. For example:

```json [settings]
{
  "theme": "sunset",
  "vim_mode": false,
  "nightly": {
    "theme": "cave-light",
    "vim_mode": true
  },
  "preview": {
    "theme": "zed-dark"
  }
}
```

With this configuration, Stable keeps all base preferences, Preview switches to `zed-dark`, and Nightly enables Vim mode with a different theme.

Changing settings in the Settings Editorwill always apply the change across all channels.

# Settings

Find below an extensive run-through of many supported settings by Zed.

## Active Pane Modifiers

- Description: Styling settings applied to the active pane.
- Setting: `active_pane_modifiers`
- Default:

```json [settings]
{
  "active_pane_modifiers": {
    "border_size": 0.0,
    "inactive_opacity": 1.0
  }
}
```

### Border size

- Description: Size of the border surrounding the active pane. When set to 0, the active pane doesn't have any border. The border is drawn inset.
- Setting: `border_size`
- Default: `0.0`

**Options**

Non-negative `float` values

### Inactive Opacity

- Description: Opacity of inactive panels. When set to 1.0, the inactive panes have the same opacity as the active one. If set to 0, the inactive panes content will not be visible at all. Values are clamped to the [0.0, 1.0] range.
- Setting: `inactive_opacity`
- Default: `1.0`

**Options**

`float` values

## Bottom Dock Layout

- Description: Control the layout of the bottom dock, relative to the left and right docks.
- Setting: `bottom_dock_layout`
- Default: `"contained"`

**Options**

1. Contain the bottom dock, giving the full height of the window to the left and right docks.

```json [settings]
{
  "bottom_dock_layout": "contained"
}
```

2. Give the bottom dock the full width of the window, truncating the left and right docks.

```json [settings]
{
  "bottom_dock_layout": "full"
}
```

3. Left align the bottom dock, truncating the left dock and giving the right dock the full height of the window.

```json [settings]
{
  "bottom_dock_layout": "left_aligned"
}
```

4. Right align the bottom dock, giving the left dock the full height of the window and truncating the right dock.

```json [settings]
{
  "bottom_dock_layout": "right_aligned"
}
```

## Agent Font Size

- Description: The font size for text in the agent panel. Inherits the UI font size if unset.
- Setting: `agent_font_size`
- Default: `null`

**Options**

`integer` values from `6` to `100` pixels (inclusive)

## Allow Rewrap

- Description: Controls where the {#action editor::Rewrap} action is allowed in the current language scope
- Setting: `allow_rewrap`
- Default: `"in_comments"`

**Options**

1. Allow rewrap in comments only:

```json [settings]
{
  "allow_rewrap": "in_comments"
}
```

2. Allow rewrap in selections only:

```json [settings]
{
  "allow_rewrap": "in_selections"
}
```

3. Allow rewrap anywhere:

```json [settings]
{
  "allow_rewrap": "anywhere"
}
```

Note: This setting has no effect in Vim mode, as rewrap is already allowed everywhere.

## Auto Indent

- Description: Whether indentation should be adjusted based on the context whilst typing. This can be specified on a per-language basis.
- Setting: `auto_indent`
- Default: `true`

**Options**

`boolean` values

## Auto Indent On Paste

- Description: Whether indentation of pasted content should be adjusted based on the context
- Setting: `auto_indent_on_paste`
- Default: `true`

**Options**

`boolean` values

## Auto Install extensions

- Description: Define extensions to be autoinstalled or never be installed.
- Setting: `auto_install_extensions`
- Default: `{ "html": true }`

**Options**

You can find the names of your currently installed extensions by listing the subfolders under the [extension installation location](./extensions/installing-extensions.md#installation-location):

On macOS:

```sh
ls ~/Library/Application\ Support/Zed/extensions/installed/
```

On Linux:

```sh
ls ~/.local/share/zed/extensions/installed
```

Define extensions which should be installed (`true`) or never installed (`false`).

```json [settings]
{
  "auto_install_extensions": {
    "html": true,
    "dockerfile": true,
    "docker-compose": false
  }
}
```

## Auto Update extensions

- Description: Define extensions to be autoupdated or manually updated.
- Setting: `auto_update_extensions`
- Default: `null`

**Options**

Define extensions which will be autoupdated (`true`) vs. those that will be manually updated (`false`).

```json [settings]
{
  "auto_update_extensions": {
    "html": true,
    "dockerfile": true,
    "docker-compose": false
  }
}
```

## Autosave

- Description: When to automatically save edited buffers.
- Setting: `autosave`
- Default: `off`

**Options**

1. To disable autosave, set it to `off`:

```json [settings]
{
  "autosave": "off"
}
```

2. To autosave when focus changes, use `on_focus_change`:

```json [settings]
{
  "autosave": "on_focus_change"
}
```

3. To autosave when the active window changes, use `on_window_change`:

```json [settings]
{
  "autosave": "on_window_change"
}
```

4. To autosave after an inactivity period, use `after_delay`:

```json [settings]
{
  "autosave": {
    "after_delay": {
      "milliseconds": 1000
    }
  }
}
```

Note that a save will be triggered when an unsaved tab is closed, even if this is earlier than the configured inactivity period.

## Autoscroll on Clicks

- Description: Whether to scroll when clicking near the edge of the visible text area.
- Setting: `autoscroll_on_clicks`
- Default: `false`

**Options**

`boolean` values

## Auto Signature Help

- Description: Show method signatures in the editor, when inside parentheses
- Setting: `auto_signature_help`
- Default: `false`

**Options**

`boolean` values

### Show Signature Help After Edits

- Description: Whether to show the signature help after completion or a bracket pair inserted. If `auto_signature_help` is enabled, this setting will be treated as enabled also.
- Setting: `show_signature_help_after_edits`
- Default: `false`

**Options**

`boolean` values

## Auto Update

- Description: Whether or not to automatically check for updates.
- Setting: `auto_update`
- Default: `true`

**Options**

`boolean` values

## Base Keymap

- Description: Base key bindings scheme. Base keymaps can be overridden with user keymaps.
- Setting: `base_keymap`
- Default: `VSCode`

**Options**

1. VS Code

```json [settings]
{
  "base_keymap": "VSCode"
}
```

2. Atom

```json [settings]
{
  "base_keymap": "Atom"
}
```

3. JetBrains

```json [settings]
{
  "base_keymap": "JetBrains"
}
```

4. None

```json [settings]
{
  "base_keymap": "None"
}
```

5. Sublime Text

```json [settings]
{
  "base_keymap": "SublimeText"
}
```

6. TextMate

```json [settings]
{
  "base_keymap": "TextMate"
}
```

## Buffer Font Family

- Description: The name of a font to use for rendering text in the editor.
- Setting: `buffer_font_family`
- Default: `.ZedMono`. This currently aliases to [Lilex](https://lilex.myrt.co).

**Options**

The name of any font family installed on the user's system, or `".ZedMono"`.

## Buffer Font Features

- Description: The OpenType features to enable for text in the editor.
- Setting: `buffer_font_features`
- Default: `null`
- Platform: macOS and Windows.

**Options**

Zed supports all OpenType features that can be enabled or disabled for a given buffer or terminal font, as well as setting values for font features.

For example, to disable font ligatures, add the following to your settings:

```json [settings]
{
  "buffer_font_features": {
    "calt": false
  }
}
```

You can also set other OpenType features, like setting `cv01` to `7`:

```json [settings]
{
  "buffer_font_features": {
    "cv01": 7
  }
}
```

## Buffer Font Fallbacks

- Description: Set the buffer text's font fallbacks, this will be merged with the platform's default fallbacks.
- Setting: `buffer_font_fallbacks`
- Default: `null`
- Platform: macOS and Windows.

**Options**

For example, to use `Nerd Font` as a fallback, add the following to your settings:

```json [settings]
{
  "buffer_font_fallbacks": ["Nerd Font"]
}
```

## Buffer Font Size

- Description: The default font size for text in the editor.
- Setting: `buffer_font_size`
- Default: `15`

**Options**

A font size from `6` to `100` pixels (inclusive)

## Buffer Font Weight

- Description: The default font weight for text in the editor.
- Setting: `buffer_font_weight`
- Default: `400`

**Options**

`integer` values between `100` and `900`

## Buffer Line Height

- Description: The default line height for text in the editor.
- Setting: `buffer_line_height`
- Default: `"comfortable"`

**Options**

`"standard"`, `"comfortable"` or `{ "custom": float }` (`1` is compact, `2` is loose)

## Centered Layout

- Description: Configuration for the centered layout mode.
- Setting: `centered_layout`
- Default:

```json [settings]
"centered_layout": {
  "left_padding": 0.2,
  "right_padding": 0.2,
}
```

**Options**

The `left_padding` and `right_padding` options define the relative width of the
left and right padding of the central pane from the workspace when the centered layout mode is activated. Valid values range is from `0` to `0.4`.

## Close on File Delete

- Description: Whether to automatically close editor tabs when their corresponding files are deleted from disk.
- Setting: `close_on_file_delete`
- Default: `false`

**Options**

`boolean` values

When enabled, this setting will automatically close tabs for files that have been deleted from the file system. This is particularly useful for workflows involving temporary or scratch files that are frequently created and deleted. When disabled (default), deleted files remain open with a strikethrough through their tab title.

Note: Dirty files (files with unsaved changes) will not be automatically closed even when this setting is enabled, ensuring you don't lose unsaved work.

## Scan Symbolic Links

- Description: When to scan content of linked directories.
- Setting: `scan_symlinks`
- Default: `never`

**Options**

- `always`: Always scan symlinked directories
- `expanded`: Only scan symlinked directories when they've been expanded in the workspace
- `never`: Never scan symlinked directories

When set to `always`, Zed will follow symbolic links and scan their contents when indexing the project. When set to `expanded`, symbolic links will only be scanned after you explicitly expand them in the project panel. When set to `never` (default), symbolic links are not followed or scanned.

## Confirm Quit

- Description: Whether or not to prompt the user to confirm before closing the application.
- Setting: `confirm_quit`
- Default: `false`

**Options**

`boolean` values

## Diagnostics Max Severity

- Description: Which level to use to filter out diagnostics displayed in the editor
- Setting: `diagnostics_max_severity`
- Default: `null`

**Options**

1. Allow all diagnostics (default):

```json [settings]
{
  "diagnostics_max_severity": "all"
}
```

2. Show only errors:

```json [settings]
{
  "diagnostics_max_severity": "error"
}
```

3. Show errors and warnings:

```json [settings]
{
  "diagnostics_max_severity": "warning"
}
```

4. Show errors, warnings, and information:

```json [settings]
{
  "diagnostics_max_severity": "info"
}
```

5. Show all including hints:

```json [settings]
{
  "diagnostics_max_severity": "hint"
}
```

## Disable AI

- Description: Whether to disable all AI features in Zed
- Setting: `disable_ai`
- Default: `false`

**Options**

`boolean` values

## Direnv Integration

- Description: Settings for [direnv](https://direnv.net/) integration. Requires `direnv` to be installed.
  `direnv` integration make it possible to use the environment variables set by a `direnv` configuration to detect some language servers in `$PATH` instead of installing them.
  It also allows for those environment variables to be used in tasks.
- Setting: `load_direnv`
- Default: `"direct"`

**Options**

There are three options to choose from:

1. `shell_hook`: Use the shell hook to load direnv. This relies on direnv to activate upon entering the directory. Supports POSIX shells and fish.
2. `direct`: Use `direnv export json` to load direnv. This will load direnv directly without relying on the shell hook and might cause some inconsistencies. This allows direnv to work with any shell.
3. `disabled`: No shell environment will be loaded automatically; direnv must be invoked manually (e.g. with `direnv exec`) to be used.

## Double Click In Multibuffer

- Description: What to do when multibuffer is double clicked in some of its excerpts (parts of singleton buffers)
- Setting: `double_click_in_multibuffer`
- Default: `"select"`

**Options**

1. Behave as a regular buffer and select the whole word (default):

```json [settings]
{
  "double_click_in_multibuffer": "select"
}
```

2. Open the excerpt clicked as a new buffer in the new tab:

```json [settings]
{
  "double_click_in_multibuffer": "open"
}
```

For the case of "open", regular selection behavior can be achieved by holding `alt` when double clicking.

## Drop Target Size

- Description: Relative size of the drop target in the editor that will open dropped file as a split pane (0-0.5). For example, 0.25 means if you drop onto the top/bottom quarter of the pane a new vertical split will be used, if you drop onto the left/right quarter of the pane a new horizontal split will be used.
- Setting: `drop_target_size`
- Default: `0.2`

**Options**

`float` values between `0` and `0.5`

## Edit Predictions

- Description: Settings for edit predictions.
- Setting: `edit_predictions`
- Default:

```json [settings]
  "edit_predictions": {
    "disabled_globs": [
      "**/.env*",
      "**/*.pem",
      "**/*.key",
      "**/*.cert",
      "**/*.crt",
      "**/.dev.vars",
      "**/secrets.yml"
    ]
  }
```

**Options**

### Disabled Globs

- Description: A list of globs for which edit predictions should be disabled for. This list adds to a pre-existing, sensible default set of globs. Any additional ones you add are combined with them.
- Setting: `disabled_globs`
- Default: `["**/.env*", "**/*.pem", "**/*.key", "**/*.cert", "**/*.crt", "**/.dev.vars", "**/secrets.yml"]`

**Options**

List of `string` values.

## Edit Predictions Disabled in

- Description: A list of language scopes in which edit predictions should be disabled.
- Setting: `edit_predictions_disabled_in`
- Default: `[]`

**Options**

List of `string` values

1. Don't show edit predictions in comments:

```json [settings]
"disabled_in": ["comment"]
```

2. Don't show edit predictions in strings and comments:

```json [settings]
"disabled_in": ["comment", "string"]
```

3. Only in Go, don't show edit predictions in strings and comments:

```json [settings]
{
  "languages": {
    "Go": {
      "edit_predictions_disabled_in": ["comment", "string"]
    }
  }
}
```

## Current Line Highlight

- Description: How to highlight the current line in the editor.
- Setting: `current_line_highlight`
- Default: `all`

**Options**

1. Don't highlight the current line:

```json [settings]
"current_line_highlight": "none"
```

2. Highlight the gutter area:

```json [settings]
"current_line_highlight": "gutter"
```

3. Highlight the editor area:

```json [settings]
"current_line_highlight": "line"
```

4. Highlight the full line:

```json [settings]
"current_line_highlight": "all"
```

## Selection Highlight

- Description: Whether to highlight all occurrences of the selected text in an editor.
- Setting: `selection_highlight`
- Default: `true`

## Rounded Selection

- Description: Whether the text selection should have rounded corners.
- Setting: `rounded_selection`
- Default: `true`

## Cursor Blink

- Description: Whether or not the cursor blinks.
- Setting: `cursor_blink`
- Default: `true`

**Options**

`boolean` values

## Cursor Shape

- Description: Cursor shape for the default editor.
- Setting: `cursor_shape`
- Default: `bar`

**Options**

1. A vertical bar:

```json [settings]
"cursor_shape": "bar"
```

2. A block that surrounds the following character:

```json [settings]
"cursor_shape": "block"
```

3. An underline / underscore that runs along the following character:

```json [settings]
"cursor_shape": "underline"
```

4. An box drawn around the following character:

```json [settings]
"cursor_shape": "hollow"
```

## Gutter

- Description: Settings for the editor gutter
- Setting: `gutter`
- Default:

```json [settings]
{
  "gutter": {
    "line_numbers": true,
    "runnables": true,
    "breakpoints": true,
    "folds": true,
    "min_line_number_digits": 4
  }
}
```

**Options**

- `line_numbers`: Whether to show line numbers in the gutter
- `runnables`: Whether to show runnable buttons in the gutter
- `breakpoints`: Whether to show breakpoints in the gutter
- `folds`: Whether to show fold buttons in the gutter
- `min_line_number_digits`: Minimum number of characters to reserve space for in the gutter

## Hide Mouse

- Description: Determines when the mouse cursor should be hidden in an editor or input box.
- Setting: `hide_mouse`
- Default: `on_typing_and_movement`

**Options**

1. Never hide the mouse cursor:

```json [settings]
"hide_mouse": "never"
```

2. Hide only when typing:

```json [settings]
"hide_mouse": "on_typing"
```

3. Hide on both typing and cursor movement:

```json [settings]
"hide_mouse": "on_typing_and_movement"
```

## Snippet Sort Order

- Description: Determines how snippets are sorted relative to other completion items.
- Setting: `snippet_sort_order`
- Default: `inline`

**Options**

1. Place snippets at the top of the completion list:

```json [settings]
"snippet_sort_order": "top"
```

2. Place snippets normally without any preference:

```json [settings]
"snippet_sort_order": "inline"
```

3. Place snippets at the bottom of the completion list:

```json [settings]
"snippet_sort_order": "bottom"
```

4. Do not show snippets in the completion list at all:

```json [settings]
"snippet_sort_order": "none"
```

## Editor Scrollbar

- Description: Whether or not to show the editor scrollbar and various elements in it.
- Setting: `scrollbar`
- Default:

```json [settings]
"scrollbar": {
  "show": "auto",
  "cursors": true,
  "git_diff": true,
  "search_results": true,
  "selected_text": true,
  "selected_symbol": true,
  "diagnostics": "all",
  "axes": {
    "horizontal": true,
    "vertical": true,
  },
},
```

### Show Mode

- Description: When to show the editor scrollbar.
- Setting: `show`
- Default: `auto`

**Options**

1. Show the scrollbar if there's important information or follow the system's configured behavior:

```json [settings]
"scrollbar": {
  "show": "auto"
}
```

2. Match the system's configured behavior:

```json [settings]
"scrollbar": {
  "show": "system"
}
```

3. Always show the scrollbar:

```json [settings]
"scrollbar": {
  "show": "always"
}
```

4. Never show the scrollbar:

```json [settings]
"scrollbar": {
  "show": "never"
}
```

### Cursor Indicators

- Description: Whether to show cursor positions in the scrollbar.
- Setting: `cursors`
- Default: `true`

Cursor indicators appear as small marks on the scrollbar showing where other collaborators' cursors are positioned in the file.

**Options**

`boolean` values

### Git Diff Indicators

- Description: Whether to show git diff indicators in the scrollbar.
- Setting: `git_diff`
- Default: `true`

Git diff indicators appear as colored marks showing lines that have been added, modified, or deleted compared to the git HEAD.

**Options**

`boolean` values

### Search Results Indicators

- Description: Whether to show buffer search results in the scrollbar.
- Setting: `search_results`
- Default: `true`

Search result indicators appear as marks showing all locations in the file where your current search query matches.

**Options**

`boolean` values

### Selected Text Indicators

- Description: Whether to show selected text occurrences in the scrollbar.
- Setting: `selected_text`
- Default: `true`

Selected text indicators appear as marks showing all occurrences of the currently selected text throughout the file.

**Options**

`boolean` values

### Selected Symbols Indicators

- Description: Whether to show selected symbol occurrences in the scrollbar.
- Setting: `selected_symbol`
- Default: `true`

Selected symbol indicators appear as marks showing all occurrences of the currently selected symbol (like a function or variable name) throughout the file.

**Options**

`boolean` values

### Diagnostics

- Description: Which diagnostic indicators to show in the scrollbar.
- Setting: `diagnostics`
- Default: `all`

Diagnostic indicators appear as colored marks showing errors, warnings, and other language server diagnostics at their corresponding line positions in the file.

**Options**

1. Show all diagnostics:

```json [settings]
{
  "show_diagnostics": "all"
}
```

2. Do not show any diagnostics:

```json [settings]
{
  "show_diagnostics": "off"
}
```

3. Show only errors:

```json [settings]
{
  "show_diagnostics": "error"
}
```

4. Show only errors and warnings:

```json [settings]
{
  "show_diagnostics": "warning"
}
```

5. Show only errors, warnings, and information:

```json [settings]
{
  "show_diagnostics": "info"
}
```

### Axes

- Description: Forcefully enable or disable the scrollbar for each axis
- Setting: `axes`
- Default:

```json [settings]
"scrollbar": {
  "axes": {
    "horizontal": true,
    "vertical": true,
  },
}
```

#### Horizontal

- Description: When false, forcefully disables the horizontal scrollbar. Otherwise, obey other settings.
- Setting: `horizontal`
- Default: `true`

**Options**

`boolean` values

#### Vertical

- Description: When false, forcefully disables the vertical scrollbar. Otherwise, obey other settings.
- Setting: `vertical`
- Default: `true`

**Options**

`boolean` values

## Minimap

- Description: Settings related to the editor's minimap, which provides an overview of your document.
- Setting: `minimap`
- Default:

```json [settings]
{
  "minimap": {
    "show": "never",
    "thumb": "always",
    "thumb_border": "left_open",
    "current_line_highlight": null
  }
}
```

### Show Mode

- Description: When to show the minimap in the editor.
- Setting: `show`
- Default: `never`

**Options**

1. Always show the minimap:

```json [settings]
{
  "show": "always"
}
```

2. Show the minimap if the editor's scrollbars are visible:

```json [settings]
{
  "show": "auto"
}
```

3. Never show the minimap:

```json [settings]
{
  "show": "never"
}
```

### Thumb Display

- Description: When to show the minimap thumb (the visible editor area) in the minimap.
- Setting: `thumb`
- Default: `always`

**Options**

1. Show the minimap thumb when hovering over the minimap:

```json [settings]
{
  "thumb": "hover"
}
```

2. Always show the minimap thumb:

```json [settings]
{
  "thumb": "always"
}
```

### Thumb Border

- Description: How the minimap thumb border should look.
- Setting: `thumb_border`
- Default: `left_open`

**Options**

1. Display a border on all sides of the thumb:

```json [settings]
{
  "thumb_border": "full"
}
```

2. Display a border on all sides except the left side:

```json [settings]
{
  "thumb_border": "left_open"
}
```

3. Display a border on all sides except the right side:

```json [settings]
{
  "thumb_border": "right_open"
}
```

4. Display a border only on the left side:

```json [settings]
{
  "thumb_border": "left_only"
}
```

5. Display the thumb without any border:

```json [settings]
{
  "thumb_border": "none"
}
```

### Current Line Highlight

- Description: How to highlight the current line in the minimap.
- Setting: `current_line_highlight`
- Default: `null`

**Options**

1. Inherit the editor's current line highlight setting:

```json [settings]
{
  "minimap": {
    "current_line_highlight": null
  }
}
```

2. Highlight the current line in the minimap:

```json [settings]
{
  "minimap": {
    "current_line_highlight": "line"
  }
}
```

or

```json [settings]
{
  "minimap": {
    "current_line_highlight": "all"
  }
}
```

3. Do not highlight the current line in the minimap:

```json [settings]
{
  "minimap": {
    "current_line_highlight": "gutter"
  }
}
```

or

```json [settings]
{
  "minimap": {
    "current_line_highlight": "none"
  }
}
```

## Editor Tab Bar

- Description: Settings related to the editor's tab bar.
- Settings: `tab_bar`
- Default:

```json [settings]
"tab_bar": {
  "show": true,
  "show_nav_history_buttons": true,
  "show_tab_bar_buttons": true
}
```

### Show

- Description: Whether or not to show the tab bar in the editor.
- Setting: `show`
- Default: `true`

**Options**

`boolean` values

### Navigation History Buttons

- Description: Whether or not to show the navigation history buttons.
- Setting: `show_nav_history_buttons`
- Default: `true`

**Options**

`boolean` values

### Tab Bar Buttons

- Description: Whether or not to show the tab bar buttons.
- Setting: `show_tab_bar_buttons`
- Default: `true`

**Options**

`boolean` values

## Editor Tabs

- Description: Configuration for the editor tabs.
- Setting: `tabs`
- Default:

```json [settings]
"tabs": {
  "close_position": "right",
  "file_icons": false,
  "git_status": false,
  "activate_on_close": "history",
  "show_close_button": "hover",
  "show_diagnostics": "off"
},
```

### Close Position

- Description: Where to display close button within a tab.
- Setting: `close_position`
- Default: `right`

**Options**

1. Display the close button on the right:

```json [settings]
{
  "close_position": "right"
}
```

2. Display the close button on the left:

```json [settings]
{
  "close_position": "left"
}
```

### File Icons

- Description: Whether to show the file icon for a tab.
- Setting: `file_icons`
- Default: `false`

### Git Status

- Description: Whether or not to show Git file status in tab.
- Setting: `git_status`
- Default: `false`

### Activate on close

- Description: What to do after closing the current tab.
- Setting: `activate_on_close`
- Default: `history`

**Options**

1.  Activate the tab that was open previously:

```json [settings]
{
  "activate_on_close": "history"
}
```

2. Activate the right neighbour tab if present:

```json [settings]
{
  "activate_on_close": "neighbour"
}
```

3. Activate the left neighbour tab if present:

```json [settings]
{
  "activate_on_close": "left_neighbour"
}
```

### Show close button

- Description: Controls the appearance behavior of the tab's close button.
- Setting: `show_close_button`
- Default: `hover`

**Options**

1.  Show it just upon hovering the tab:

```json [settings]
{
  "show_close_button": "hover"
}
```

2. Show it persistently:

```json [settings]
{
  "show_close_button": "always"
}
```

3. Never show it, even if hovering it:

```json [settings]
{
  "show_close_button": "hidden"
}
```

### Show Diagnostics

- Description: Whether to show diagnostics indicators in tabs. This setting only works when file icons are active and controls which files with diagnostic issues to mark.
- Setting: `show_diagnostics`
- Default: `off`

**Options**

1. Do not mark any files:

```json [settings]
{
  "show_diagnostics": "off"
}
```

2. Only mark files with errors:

```json [settings]
{
  "show_diagnostics": "errors"
}
```

3. Mark files with errors and warnings:

```json [settings]
{
  "show_diagnostics": "all"
}
```

### Show Inline Code Actions

- Description: Whether to show code action button at start of buffer line.
- Setting: `inline_code_actions`
- Default: `true`

**Options**

`boolean` values

### Session

- Description: Controls Zed lifecycle-related behavior.
- Setting: `session`
- Default:

```json
{
  "session": {
    "restore_unsaved_buffers": true,
    "trust_all_worktrees": false
  }
}
```

**Options**

1.  Whether or not to restore unsaved buffers on restart:

```json [settings]
{
  "session": {
    "restore_unsaved_buffers": true
  }
}
```

If this is true, user won't be prompted whether to save/discard dirty files when closing the application.

2. Whether or not to skip worktree and workspace trust checks:

```json [settings]
{
  "session": {
    "trust_all_worktrees": false
  }
}
```

When trusted, project settings are synchronized automatically, language and MCP servers are downloaded and started automatically.

### Drag And Drop Selection

- Description: Whether to allow drag and drop text selection in buffer. `delay` is the milliseconds that must elapse before drag and drop is allowed. Otherwise, a new text selection is created.
- Setting: `drag_and_drop_selection`
- Default:

```json [settings]
"drag_and_drop_selection": {
  "enabled": true,
  "delay": 300
}
```

## Editor Toolbar

- Description: Whether or not to show various elements in the editor toolbar.
- Setting: `toolbar`
- Default:

```json [settings]
"toolbar": {
  "breadcrumbs": true,
  "quick_actions": true,
  "selections_menu": true,
  "agent_review": true,
  "code_actions": false
},
```

**Options**

Each option controls displaying of a particular toolbar element. If all elements are hidden, the editor toolbar is not displayed.

## Use System Tabs

- Description: Whether to allow windows to tab together based on the user’s tabbing preference (macOS only).
- Setting: `use_system_window_tabs`
- Default: `false`

**Options**

This setting enables integration with macOS’s native window tabbing feature. When set to `true`, Zed windows can be grouped together as tabs in a single macOS window, following the system-wide tabbing preferences set by the user (such as "Always", "In Full Screen", or "Never"). This setting is only available on macOS.

## Enable Language Server

- Description: Whether or not to use language servers to provide code intelligence.
- Setting: `enable_language_server`
- Default: `true`

**Options**

`boolean` values

## Ensure Final Newline On Save

- Description: Removes any lines containing only whitespace at the end of the file and ensures just one newline at the end.
- Setting: `ensure_final_newline_on_save`
- Default: `true`

**Options**

`boolean` values

## Expand Excerpt Lines

- Description: The default number of lines to expand excerpts in the multibuffer by
- Setting: `expand_excerpt_lines`
- Default: `5`

**Options**

Positive `integer` values

## Excerpt Context Lines

- Description: The number of lines of context to provide when showing excerpts in the multibuffer.
- Setting: `excerpt_context_lines`
- Default: `2`

**Options**

Positive `integer` value between 1 and 32. Values outside of this range will be clamped to this range.

## Extend Comment On Newline

- Description: Whether to start a new line with a comment when a previous line is a comment as well.
- Setting: `extend_comment_on_newline`
- Default: `true`

**Options**

`boolean` values

## Extend List On Newline

- Description: Whether to continue lists when pressing Enter at the end of a list item. Supports unordered, ordered, and task lists. Pressing Enter on an empty list item removes the marker and exits the list.
- Setting: `extend_list_on_newline`
- Default: `true`

**Options**

`boolean` values

## Indent List On Tab

- Description: Whether to indent list items when pressing Tab on a line containing only a list marker. This enables quick creation of nested lists.
- Setting: `indent_list_on_tab`
- Default: `true`

**Options**

`boolean` values

## Status Bar

- Description: Control various elements in the status bar. Note that some items in the status bar have their own settings set elsewhere.
- Setting: `status_bar`
- Default:

```json [settings]
"status_bar": {
  "active_language_button": true,
  "cursor_position_button": true,
  "line_endings_button": false,
  "active_encoding_button": "non_utf8"
},
```

There is an experimental setting that completely hides the status bar. This causes major usability problems (you will be unable to use many of Zed's features), but is provided for those who value screen real-estate above all else.

```json
"status_bar": {
  "experimental.show": false
}
```

## LSP

- Description: Configuration for language servers.
- Setting: `lsp`
- Default: `null`

**Options**

The following settings can be overridden for specific language servers:

- `initialization_options`
- `settings`

To override configuration for a language server, add an entry for that language server's name to the `lsp` value.

Some options are passed via `initialization_options` to the language server. These are for options which must be specified at language server startup and when changed will require restarting the language server.

For example to pass the `check` option to `rust-analyzer`, use the following configuration:

```json [settings]
"lsp": {
  "rust-analyzer": {
    "initialization_options": {
      "check": {
        "command": "clippy" // rust-analyzer.check.command (default: "check")
      }
    }
  }
}
```

While other options may be changed at a runtime and should be placed under `settings`:

```json [settings]
"lsp": {
  "yaml-language-server": {
    "settings": {
      "yaml": {
        "keyOrdering": true // Enforces alphabetical ordering of keys in maps
      }
    }
  }
}
```

## Global LSP Settings

- Description: Configuration for global LSP settings that apply to all language servers
- Setting: `global_lsp_settings`
- Default:

```json [settings]
{
  "global_lsp_settings": {
    "button": true
  }
}
```

**Options**

- `button`: Whether to show the LSP status button in the status bar

## LSP Highlight Debounce

- Description: The debounce delay in milliseconds before querying highlights from the language server based on the current cursor location.
- Setting: `lsp_highlight_debounce`
- Default: `75`

**Options**

`integer` values representing milliseconds

## Features

- Description: Features that can be globally enabled or disabled
- Setting: `features`
- Default:

```json [settings]
{
  "features": {
    "edit_prediction_provider": "zed"
  }
}
```

### Edit Prediction Provider

- Description: Which edit prediction provider to use
- Setting: `edit_prediction_provider`
- Default: `"zed"`

**Options**

1. Use Zeta as the edit prediction provider:

```json [settings]
{
  "features": {
    "edit_prediction_provider": "zed"
  }
}
```

2. Use Copilot as the edit prediction provider:

```json [settings]
{
  "features": {
    "edit_prediction_provider": "copilot"
  }
}
```

3. Use Supermaven as the edit prediction provider:

```json [settings]
{
  "features": {
    "edit_prediction_provider": "supermaven"
  }
}
```

4. Turn off edit predictions across all providers

```json [settings]
{
  "features": {
    "edit_prediction_provider": "none"
  }
}
```

## Format On Save

- Description: Whether or not to perform a buffer format before saving.
- Setting: `format_on_save`
- Default: `on`

**Options**

1. `on`, enables format on save obeying `formatter` setting:

```json [settings]
// .zed/settings.json
{
  "tab_size": 2,
  "formatter": "prettier",
  "format_on_save": "on"
}
```

You can also add settings files in subdirectories for more granular control.

**Limitation:** Not all settings can be set at the project level. Settings that affect the editor globally (like `theme` or `vim_mode`) only work in user settings. Project settings are limited to editor behavior and language tooling options like `tab_size`, `formatter`, and `format_on_save`.

## How Settings Merge

Settings are applied in layers:

1. **Default settings** — Zed's built-in defaults
2. **User settings** — Your global preferences
3. **Project settings** — Project-specific overrides

Later layers override earlier ones. For object settings (like `terminal`), properties merge rather than replace entirely.

## Per-Release Channel Overrides

Use different settings for Stable, Preview, or Nightly builds by adding top-level channel keys:

```json [settings]
{
  "theme": "One Dark",
  "vim_mode": false,
  "nightly": {
    "theme": "Rosé Pine",
    "vim_mode": true
  },
  "preview": {
    "theme": "Catppuccin Mocha"
  }
}
```

With this configuration:

- **Stable** uses One Dark with vim mode off
- **Preview** uses Catppuccin Mocha with vim mode off
- **Nightly** uses Rosé Pine with vim mode on

Changes made in the Settings Editor apply across all channels.

## Settings Deep Links

Zed supports deep links that open specific settings directly:

```
zed://settings/theme
zed://settings/vim_mode
zed://settings/buffer_font_size
```

These are useful for sharing configuration tips or linking from documentation.

## Example Configuration

```json [settings]
{
  "theme": {
    "mode": "system",
    "light": "One Light",
    "dark": "One Dark"
  },
  "buffer_font_family": "JetBrains Mono",
  "buffer_font_size": 14,
  "tab_size": 2,
  "format_on_save": "on",
  "autosave": "on_focus_change",
  "vim_mode": false,
  "terminal": {
    "font_family": "JetBrains Mono",
    "font_size": 14
  },
  "languages": {
    "Python": {
      "tab_size": 4
    }
  }
}
```

## What's Next

- [Appearance](./appearance.md) — Themes, fonts, and visual customization
- [Key bindings](./key-bindings.md) — Customize keyboard shortcuts
- [AI Configuration](./ai/configuration.md) — Set up AI providers, models, and agent settings
- [All Settings](./reference/all-settings.md) — Complete settings reference
