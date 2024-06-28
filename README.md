# Plover undo tools
Commands that modify how translations behave when Plover undoes them.


## Setup
This plugin is currently not available from Plover's Plugins Manager. [Instructions are available on the Plover wiki](https://plover.wiki/index.php/Plugins#Find_the_plugin_on_PyPI_or_as_a_git_repo) on how to install this plugin from this Git repository.


## Commands


### `{plover:undoable}`
Makes a translation that does not produce any text output undoable.

For instance, to make various spacing commands undoable:
* `"TK-LS": "{plover:undoable}{^}"`
* `"KPA": "{plover:undoable}{~|}{-|}"`
* `"KPA*": "{plover:undoable}{^}{-|}"`
* `"KPWAOEGS": "{plover:undoable}{^}{MODE:LOWER}{MODE:SET_SPACE:-}"`

With these entries, stroking e.g. `KPA/*` will no longer undo the last stroke before `KPA` that caused text to be output (what happens by default in Plover); it will instead not alter the currently typed text at all, but now the next word will no longer be capitalized.


#### Edge case behavior
* If used on an entry that is normally undoable, an extra undo is needed before Plover will start undoing the translation before it.


### `{plover:undo_with:...}`
Causes additional text or commands to be translated when a given translation is undone.

Some example use cases:
* Undoing `{plover:solo_dict}` command calls:<br />`"PHRAU": "{plover:undo_with:\\{plover:end_solo_dict\\}}{plover:solo_dict:+raw-solo-dict.json}{^ ^}{MODE:SET_SPACE:/}"`
* Undoing movement strokes:<br />`"STPH-G": "{#right}{^}{plover:undo_with:\\{#left\\}}"`


#### Edge case behavior
* (TODO?) The number of undos needed to erase a multistroke translation that uses this command may not match the number of strokes in the entry. If this command is set to produce text, the entire translation is replaced with text that can be erased with just 1 stroke.