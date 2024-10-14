CONFIGURATIONS
Every configuration has a default value embedded into the script itself.
A list of all configurations:
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
        
CONSTANT CONFIGURATIONS
Constant configurations are configurations whose default values can't be changed. This is to ensure that the user does not change a vital configuration value that would break the script, which would require the user to manually repair the system.
DIR_MAIN, DIR_BKUP, FILE_CONFIG, FILE_HELP, TAR_MP3S, TAR_PLAY, EXT_MP3S, EXT_PLAY, EXT_BKUP, SEP_MONO, USER_INPUT are constant configurations.
MUTABLE CONFIGURATIONS
Mutable configurations configurations are configurations whose default values can be changed. This is done by appending a properly formatted line assigning a new value to a configuration name to FILE_CONFIG. Although this can be done manually, it is strongly recommended to do it through the script interface of CONFIG SET.
DIR_PLAY, DIR_MP3S, SEP_AUAU and SEP_AUMP are mutable configurations.

DEFAULTS
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

The script asserts a convention for naming mp3 files and playlists:
(author)(SEP_AUAU)(author...)(SEP_AUMP)(song-name)(EXT_MP3S)    "Jason Derulo - Acapulco.mp3"
(title)(EXT_PLAY)                                               "jason-derulo.plst"

Note about separators: apparently bash hates when something ends with white characters, therefore the default values of separators are outputted with trailings removed. This means that the default visible output of separators is not ", " and " - ", and instead is outputted as "," and " -". This is however a visual glitch, and does not affect the inner workings of the script.

EXTERNAL SOFTWARE
mpg123 and sed

COMMANDS

HELP
    no options
    output a list of all available commands

PLAYLIST
    option for manipulating playlists
add
    options:
    1) playlist
    2...) mp3
    for each MP3-FILE, add it to PLAYLIST. If MP3-FILE already in PLAYLIST, omit adding(no more than 1 entry allowed).
del
    options:
    1) playlist
    2...) mp3
    for each MP3-FILE, delete it from PLAYLIST. If MP3-FILE not in PLAYLIST, omit deletion.

Pro tip: when adding/deleting MP3-FILES to PLAYLIST, change current working directory to your mp3 directory, as this will allow bash to expand mp3 file names with tabs or wildcards
del

create
    options:
    1...) playlist
    for each PLALIST, create a blank playlist of that name. If playlist of that name already exist, omit its creation.

destroy
    options:
    1...) playlist
    for each PLAYLIST, destroy the playlist(delete it). If playlist of that name does not exist, omit its destruction.
    
copy 
    options:
    1) existing playlist name
    2) new playlist name
    Copy existing playlist name to a file called new playlist name

play
    options:
    1) playlist
    2) keyword [ random | original ]
    Play music from playlist:
        original will preserve the order of which the mp3 files are given in the playlist.
        random will shuffle the order of mp3 files. Every mp3 file will be outputted only once.
SHOW
    option for showing information
authors
    no options 
    List every author with the number of mp3 files they are author or co-author of. Denoting 1 mp3 file authorship is omitted.
authors 
    options:
    1...) mp3
    For every mp3 file song name list its authors
playlists
    no options
    List every playlist.

mp3s
    options:
    1) keyword [ all | used | unused ]
    list mp3 files:
        "all" lists all available mp3 files
        "used" outputs only mp3 files that are used inside a playlist
        "unused" outputs only mp3 files that are unused in any playlist
    
author
    options:
    1...) author
    For every author list every mp3 file they are author of.

playlist
    options:
    1...) playlist
    for every playlist list its contents.

mp3
    options:
    1...) mp3
    for every mp3 file list the playlists that contain in.

CHECK
mp3s
    no options.
    check mp3 directory files for correct naming format.

playlists
    no options.
    check playlists directory files for correct naming format.

playlist
    options:
    1...) playlist
    for every playlist, check entries for entries pointing to non-existant mp3 files.
    # important: playlist default name format, mp3 default name format, 

BACKUP
As of now, a backup will always overwrite the last backup, so there can only be 1 backup of anything.
mp3s
    no options
    archive mp3 directory to TAR_MP3S.
playlists
    no options
    archive playlists directory to TAR_PLAY.
configs
    no options
    archive FILE_CONFIG to DIR_BKUP
# important: backup directory, config file, playlist directory, mp3s directory, backup files
# backup overwrites last backup

RESTORE
mp3s
    no options
    restore TAR_MP3S into DIR_MP3S
playlists
    no options
    restore TAR_PLAY into DIR_PLAY
configs
    no options
    restore FILE_CONFIG from DIR_BKUP
all
    no options
    restore configs, playlists and mp3s (in that order)
# restore overwrites current configs, and mp3s & playlists of the same name

CONFIG
configure
    no options
    Configure the system for running the app. This will create DIR_MAIN, DIR_BKUP, FILE_HELP, FILE_CONFIG, DIR_MP3S, DIR_PLAY and check if sed and mpg123 is installed.

get
    options:
    1...) config-name
    For config-name output it with its value.

list
    no options:
    list every config-name with its value

set
    options:
    1) config-name
    2) value
    Change config-name value to the new value. This is done by appending a configuration assigning entry to FILE_CONFIG.

unset
    options:
    1...) config-name
    Delete value assigning entry from FILE_CONFIG, effectively restoring the default configuration value.
