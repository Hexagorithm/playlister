# PLAYLISTER
### A Mp3 Playlist Manager
## Idea
### A Problem
In a world of disagreement and discussion, where man can be deemed worthy of eternal torture because of the type of TV show he likes, I believe there is only one truth we all can unanimously agree on - music slaps! Most mortals choose to listen to music on online platforms like Spotify, Apple Music or even Youtube.

Yet **_the chosen few_** decide to directly download music as mp3 files and store them all inside one directory. This _enlightenment_, however, comes at a **cost**. While most if not all online platforms have their own ~~sophisticated~~ tech and methods to organize music into playlists, a native system lacks these capabilites, leaving little to no organization. Yet fear not, music downloader, for this is the app that will free you of that burden!
### The Solution
Playlister is a **Linux text-based CLI** app for managing playlists of mp3 files. Assuming there exists a mp3 filled directory on your system, you can now easily create clear text lists of songs you can then pipe right into a `mpg123`. 

What is more, its AI free (who knew that one could, instead of patching design flaws with an ai product, just make a good app to begin with?) !
### Why bash?
Since this app is a glorofied list of aliases automating simple file operations, `bash` felt like the tool for the job. I have also picked bash because of its popularity and speed of development. 

I also believe `bash` to be a good step up from pseudo code I wrote for the project, and count on it to offload the complexity of implemeting this same program in more general purpose languages like *Python* (or *C* ... :skull:).
## The Program
### Installing
This script utilizes the `sed` command for file content manipulation, be sure you have `sed` installed on your system.
On Debian-based systems:

```bash
sudo apt install sed
```

This script also utilizes `mpg123` to output mp3 files as music, which should also be available.
On Debian-based systems:

```bash
sudo apt install mpg321 # mpg123 installs with mpg321
```

While a modification of the music outputting medium is *possible*, it would involve editing the source code itself. The script has also been designed with only `mpg123` in mind, and should **not** be expected to work with other command-line music players.
	
After git cloning or downloading a zip file of the project in order to get the script and needed files, the first command you should run is:

```bash
./playlister config configure
```

This command will set up your environment for usage of `playlister`.
	
The next thing in business is to copy the script into a directory listed in the systems `PATH` variable, so that playlister can be executed from *any* directory in the system. `/usr/local/bin/` is **recommended**.

```bash
sudo cp playlister /usr/local/bin/	# adding a file to /usr/local/bin requires root privliges
playlister				# check if playlister is found in PATH (should return "no options given!" error)
```

The `playlister` file located in the current directory can be removed after being copied to `/USR/LOCAL/BIN/` and after the user checked if the script can be executed successfully.
	
### Usage

Here are the **most frequently** used `playlister` commands:

```bash
playlister config set DIR_MP3S /path/to/mp3/directory			# tell playlister where mp3s are
playlister playlist create myplaylist.plst				        # create a plalist
playlister playlist add myplaylist.plst "LORN - Anvil.mp3"	    # add an entry to a playlist
playlister playlist del myplaylist.plst "LORN - Anvil.mp3"	    # del(ete) an entry from a playlist
playlister playlist play myplaylist.plst random			        # play a playlist randomly
```

For an *in-depth* look at the usage of `playliser`, please reffer to **USAGE.md**.
### Uninstalling
The script does **not** handle uninstalling of itself, which means that the user has to wipe his system **himself**.

1. Locate the mutable directories containing playlists and mp3 files.
To do this, run:

```bash
playlister config list
```

Look for `DIR_MP3S` and `DIR_PLAY`. If the values point to directories that are not children of the `MAIN_DIR` directory, the user has to delete them manually (if he so chooses).
2. Recursively remove the main `.PLAYLISTER` directory:

```bash
rm -r <user's home directory>/.playlister/
```

3. Delete the `playlister` script in `/usr/local/bin/`, where he previously copied the script to:

```bash
sudo rm /usr/local/bin/playlister		# removing files from /usr/local/bin requres root
```

**_Remember, live off the grid!_**
https://www.theverge.com/24257157/youtube-sesac-music-licensing-streaming
