# gaz
### simple shell password manager

### features

- written in safe and shellcheck compliant posix sh
- only 126 loc minus blank lines and comments
- compatible with pass's password store
- clears the clipboard after a configurable timeout
- configurable password generation using /dev/random
- guards against set -x, ps, and /proc leakage
- easily extendible through the shell

### dependencies

- gpg or gpg2
- xclip (optional for clipboard support)

### usage

```
gaz 1.0.0 - simple password manager

usage:
	-	[a]dd  [name] - create a new password entry
	-	[c]opy [name] - copy entry to the clipboard
	-	[d]el  [name] - delete a password entry
	-	[l]ist		  - list all entries
	- 	[s]how [name] - show password for an entry
	-	[t]ree		  - list all entries in a tree

using a key pair:	export GAZ_KEYID=XXXXXXXX
password length:	export GAZ_LENGTH=50
password pattern:	export GAZ_PATTERN=_A-Z-a-z-0-9
store location:		export GAZ_DIR=~/.gaz
clipboard tool:		export GAZ_CLIP='xclip -sel c'
clipboard timeout:	export GAZ_TIMEOUT=15 ('off' to disable')
```

### questions you may have

### how does this differ from other cli password managers?
gaz is written in posix sh and is shellcheck compliant. it isn't overly complex and is only 126 loc minus blank lines and comments.

### where are passwords stored?
the passwords are stored in gpg encrypted files at ~/.gaz by default.
you can change it by doing `export GAZ_DIR=[directory].

### how can i use a public key?
set the environment variable GAZ_KEYID to the ID of the key you'd like to encrypt and decrypt passwords with.

example:

```
# default null
export GAZ_KEYID=XXXXXXXX

# this can also be an email
export GAZ_KEYID=lainplus@waifu.club

# this can also be used as a one off
GAZ_KEYID=XXXXXXXX gaz add github
```

### how do i change the max password length?
set GAZ_LENGTH to a valid integer

example:

```
# default 50
export GAZ_LENGTH=8008135

# this can also be used as a one off
GAZ_LENGTH=12 gaz add gitlab
```

### how do i change the password generation pattern?
set GAZ_PATTERN to a valid tr string

example:

```
# default '_A-Z-a-z-0-9'
export GAZ_PATTERN=_A-D-e-q-0-9

# this can also be used as a one off
GAZ_PATTERN=_A-D-e-q-0-9 gaz add obscureforum
```

### how do i change the stored passwords' locations?
see `where are passwords stored?`

### how do i change the clipboard tool?
set GAZ_CLIP to a command

i advise that you disable clipboard history in managers like KDE's klipper before copying passwords through gaz. your de's clipboard manager may read entries from the X clipboard when xclip is used

```
# default xclip -sel c
export GAZ_CLIP='xclip -sel c'

# this can also be used as a one-off
GAZ_CLIP='xclip -sel c' gaz copy github
```

### how do i change the clipboard timeout?
set GAZ_TIMEOUT to a valid sleep interval (or off to disable the timeout)

```
# default 15
export GAZ_TIMEOUT=15

# disable
export GAZ_TIMEOUT=off

# what do you think
GAZ_TIMEOUT=5 gaz copy github
```

### how do i rename an entry?
it's a file. what do you think?

### how can i extend gaz?
a shell function can be used to add new commands and functionality to gaz. the following example adds gaz git to execute git commands on the password store.

```sh
gaz() {
	case $1 in
		g*)
			cd "${GAZ_DIR:=${XDG_DATA_HOME:=$HOME}/.gaz}"
			shift
			git "$@'
		;;
		
		*)
			command gaz "$@"
		;;
	esac
}
```

enjoy yourself
