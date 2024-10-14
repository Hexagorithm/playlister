# USAGE
## CONFIGURATIONS
Every configuration has a default value embedded into the script itself.
### A list of all configurations:

```bash
DIR_MAIN    # main absolute path directory.
DIR_BKUP    # backup absolute path directory.
DIR_MP3S    # mp3 files absolute path directory.
DIR_PLAY    # playlist absolute path directory.
FILE_CONFIG # configuration file absolute path (here mutable configurations can be changed).
FILE_HELP   # help file absolute path.
TAR_MP3S    # absolute path to archived mp3s
TAR_PLAY    # absolute path to archived playlists
EXT_MP3S    # extension of mp3 files
EXT_PLAY    # extension of playlist files
EXT_BKUP    # extension of backup files
SEP_AUAU    # single/multichar separator between authors in mp3 filename.
SEP_AUMP    # single/multichar separator between authors and the mp3 name in mp3 filename.
SEP_MONO    # processing constant (dont worry about it)
USER_INPUT  # user input prompt string
```
       
### CONSTANT CONFIGURATIONS
Constant configurations are configurations whose default values can't be changed. This is to ensure that the user does not change a vital configuration value that would break the script, which would require the user to manually repair the system.

`DIR_MAIN`, `DIR_BKUP`, `FILE_CONFIG`, `FILE_HELP`, `TAR_MP3S`, `TAR_PLAY`, `EXT_MP3S`, `EXT_PLAY`, `EXT_BKUP`, `SEP_MONO`, `USER_INPUT` are constant configurations.
### MUTABLE CONFIGURATIONS
Mutable configurations configurations are configurations whose default values can be changed. This is done by appending a properly formatted line assigning a new value to a configuration name to `FILE_CONFIG`. Although this can be done manually, it is strongly recommended to do it through the script interface of `config set`.

`DIR_PLAY`, `DIR_MP3S`, `SEP_AUAU` and `SEP_AUMP` are mutable configurations.

## DEFAULTS

```bash
DIR_MAIN    [user-home-directory]/.playlister
DIR_BKUP    [DIR_MAIN]/backups
DIR_MP3S    [DIR_MAIN]/mp3s
DIR_PLAY    [DIR_MAIN]/playlists
FILE_CONFIG [DIR_MAIN]/playlister.config
FILE_HELP   [DIR_MAIN]/playlister.help
TAR_MP3S    [DIR_MAIN]/mp3s.tar
TAR_PLAY    [DIR_MAIN]/playlists.tar
EXT_MP3S    ".mp3s"
EXT_PLAY    ".plst"
EXT_BKUP    ".bak"
SEP_AUAU    ", "
SEP_AUMP    " - "
SEP_MONO    "|"
USER_INPUT  ":"
```

The script asserts a convention for naming mp3 files and playlists:

```bash
[<author><SEP_AUAU><author...>]<SEP_AUMP><song-name><EXT_MP3S>    "LORN - Anvil.mp3"
<title><EXT_PLAY>                                               "lorn-hits.plst"
```

In the case of `playlister show authors` a distinction must be made.
A "mp3" is the full name of the file, following the mentioned format.
A "mp3name" is the full name of the file, stripped of the authors and `SEP_AUMP` separator:
"mp3" -> "LORN - Anvil.mp3"
"mp3name" -> Anvil.mp3

Note about separators: apparently bash hates when something ends with white characters, therefore the default values of separators are outputted with trailings removed. This means that the default visible output of SEP_AUMP is not `" - "`, but `" -"`. This is however a visual glitch, and does not affect the inner workings of the script.

## EXTERNAL SOFTWARE
`playlister` requires external software to function, that being `mpg123` and `sed` (is sed external software?).

On Debian:

```bash
sudo apt install sed
sudo apt install mpg321 # mpg123 installs with mpg321
````

## COMMANDS

### help

```bash
playlister help
```

List of all available commands with a short description. This is actually listing the contents of `FILE_HELP`.

### playlist
Manipulate playlists.

#### add

```bash
playlister playlist add <playlist> <mp3...>
```

For each `<mp3>` given, add it to `<playlist>`. If `<mp3>` already in `<playlist>`, omit adding (playlist entries can't repeat).

#### del

```bash
playlister playlist del <playlist> <mp3...>
```

For each `<mp3>` given, del(ete) it from `<playlist>`. Omits if `<mp3>` not in `<playlist>`.

Pro tip: when adding/deleting `<mp3>` to/from `<playlist>`, change current working directory to your `DIR_MP3S`, as this will allow `bash` to expand `<mp3>` file names with tabs or wildcards, saving you from typing the full name.

#### create

```bash
playlister playlist create <playlist...>
```

For each `<playlist>`, create a blank playlist of that name. Omits if `<playlist>` already exists.

####  destroy

```bash
playlister playlist destroy <playlist...>
```

For each `<playlist>`, destroy (delete) it. Omits if `<playlist>` of that name does not exist.

#### copy

```bash
playlister playlist copy <playlist> <new-playlist>
```

Copy `<playlist>` contents to new playlist `<new-playlist>`. Fails if `<new-playlist>` points to existing playlist.

#### play

```bash
playlister playlist play <playlist> [ random  | original ]
```

Play music from playlist:
- `original` - preserve the order of which the mp3 files are given in the `<playlist>`.
- `random` - shuffle the order of mp3 files.
Every mp3 file will be outputted only once.

### show
Show information.

#### authors (1)

```bash
playlister show authors
```

List every author with the number of mp3 files they are author or co-author of. Denoting 1 mp3 file authorship is omitted.

#### authors (2)

```bash
playlister show authors <mp3name...>
```

For every `<mp3name>` list its authors. See [DEFAULTS](#DEFAULTS)

#### playlists

```bash
playlister show playlists
```

Show every available playlist.

#### mp3s

```bash
playlister show mp3s [ all | used | unused ]
```

List mp3 files:
- `all` - all available mp3 files (a listing of `DIR_MP3S`)
- `used` - only mp3 files that are used by a playlist or more
- `unused` - only mp3 files that are not used in any playlist
    
#### author

```bash
playlister show author <author...>
```

Lists every mp3name the `<author>` is author of. See [DEFAULTS](#DEFAULTS)

#### playlist

```bash
playlister show playlist <playlist...>
```

For every `<playlist>` list its contents.

#### mp3

```bash
playlister show mp3 <mp3...>
```

For every mp3 file list the playlists that contain in.

### check

Check for correct name format.

#### mp3s

```bash
playlister check mp3s
```

Check mp3 directory files for correct naming format.

#### playlists

```bash
playlister check playlists
```

Check playlists directory files for correct naming format.

#### playlist

```bash
playlister check playlist <plalist...>
```

For every playlist, check entries for entries pointing to non-existant mp3 files. 

### backup
Backup data.

As of now, a backup will always overwrite the last backup, so there can only be 1 backup of anything.
#### mp3s

```bash
playlister backup mp3s
```

Archive `DIR_MP3S` to `TAR_MP3S`.

#### playlists

```bash
playlister backup playlists
```

Archive `DIR_PLAY` directory to `TAR_PLAY`.

#### configs

```bash
playlister backup configs
```

Archive `FILE_CONFIG` to `DIR_BKUP`.

#### all

```bash
playlister backup all
```
Archive:
1. `FILE_CONFIG`
2. `DIR_PLAY`
3. `DIR_MP3S`

(in that order).

### restore
Restore data from backup.

`restore` will overwrite any file that has the same name as a file being restored, so e.g. `FILE_CONFIG` will be overwritten.

#### mp3s

```bash
playlister restore mp3s
```

Restore `TAR_MP3S` into `DIR_MP3S`.


#### playlists

```bash
playlister restore playlists
```

Restore `TAR_PLAY` into `DIR_PLAY`.

#### configs

```bash
playlister restore configs
```

Restore `FILE_CONFIG` from `DIR_BKUP`.

#### all

```bash
playlister restore all
```
Restore:
1. `FILE_CONFIG`
2. `DIR_PLAY`
3. `DIR_MP3S`

(in that order).

### config

Configure environment.

#### configure

```bash
playlister config configure
```

Configure the system for running the app: 
1. create `DIR_MAIN`
2. create `DIR_BKUP`
3. create `FILE_HELP`
4. create `FILE_CONFIG`
5. create `DIR_MP3S`
6. create `DIR_PLAY`
7. check if `sed` is available
8. check if `mpg123` is available.

#### get

```bash
playlister config get <config-name...>
```

For each `<config-name>` output it with its current value.


#### list

```bash
playlister config list
```

List every configuration with its current value.

#### set

```bash
playlister config set <config-name> <new-value>
```

Change `<config-name>` to have new value `<new-value>`. This is done by appending a configuration assigning entry to `FILE_CONFIG`. This will fail if `<config-name>` reffers to an invalid or unmutable config. See [MUTABLE CONFIGURATIONS](#MUTABLE-CONFIGURATIONS)

#### unset

```bash
playlister config unset <config-name...>
```
unset

Delete value assigning entry from `FILE_CONFIG`, effectively restoring the default configuration value of `<config-name>`.
