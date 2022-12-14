# Arch Linux Note  

This is my learning note of all kinds of things as I'm trying to be familiar with Linux as a rookie. Some contents may not be written in detail as I'm already familiar with relevent concepts. So the learning note is somewhat personalized. Also note that some materials are cited from some books/websites. Because of my laziness I won't cite them in this note but I do appreciate all of these materials.

## Pacman  

CMD                     | Description
-----------------       | -------------
```pacman -Rs```        | Remove package and dependency 
```pacman -Ss <name>``` | Search remote package
```pacman -Q <name>```  | Search local package
```pacman -Q```         | List all local packages
```pacman -Qs ```       | Detailed search local packages
```pacman -Qi <name>``` | Extensive search local packages

pacman mirrorlist: `/etc/pacman.d/mirrorlist`  
`pacman -Syyu` to sync modified mirrolist  
pacman configuration file: `/etc/pacman.conf`

## systemmd  

PID 1, system and service manager  
Units: service, mount, device, socket  

CMD                                   | DESCRIPTION
--                                    | --
`systemctl status`                    |
`systemctl list-units`                |
`systemctl list-units --type=service` | list service only
`systemctl start unit`                | start a unit immediately
`systemctl stop unit`                 | stop a unit immediately
`systemctl enable unit`               | start automatically at boot
`systemctl enable --now unit`         | start automatically at boot and start it now
`systemctl disable unit`              | no longer start at boot
`systemctl mask unit`                 | impossible to start
`systemctl unmask unit`               |


## Environment Variables  

CMD                             | Description
--                              | --
`printenv <var>`                | print one variable
`printenv`                      | print all environment variables
`set`                           | print all variables(including shell var etc)
`set <myvar>`                   | set a var(not env)
`export <myvar>`                | set <myvar> to env, only change env var of my child processes
`export MYVAR=content`          | directly set env var (temporary)
add env var in /etc/environment | persistent env var(system-wide)
add env var in /etc/profile     | persistent env var(for this profile)

Environment varialbe $PATH: list of directories to be searched when executing command  

In command-line, you can add `VAR=value` in front of the command to temporarily change the environmental variable for this command. Example:`LANG=C ls .`



## DWM  
Require:  st, dmenu, dwm  
Use git clone and make install
/usr/share/xsessions/dwm.desktop content:  
```
[Desktop Entry]
Encoding=UTF-8
Name=dwm
Comment=Dynamic Window Manager  
Exec=dwm
Icon=dwm
Type=xsession

```

Shortcut                    | Description  
--                          | --
shift+alt+enter             | open st
shift+alt+c                 | close window
shift+alt+q                 | quit dwm
alt+j/k                     | change window
alt+p                       | open dmenu
alt+enter                   | set current window as master window  
alt+m/t                     | change full screen
alt+1-9                     | change tag             

Apply patches 'patch --merge -i <patch.diff>'


## xorg



### xinit
Used to start window managers  
Default configuration file: `etc/X11/xinit/xinitrc`  
Configuration file in home directory :`~/.xinitrc`  
Note commands after `exec` won't be executed  

To start, run `startx` or set auto start 

#### Autostart
Add the following in `~/.bash_profile`
```
if [-z "${DISPLAY}" && ["${XDG_VTNR}" -eq 1]] then
    exec startx
fi
```


### xorg-xinput  
A tool to manage input devices
CMD                             | DEScription  
--                              | --  
xinput list                     | show devices


## libinput  
A library to handle input devices  
Default configuration file is in `/usr/share/X11/xorg.conf.d/40-libinput.conf`

CMD                             | Description
--                              | --  
libinput list-devices           | show devices

**Note**: this method can't change touchpad to natural scrolling for my laptop

## xf86-input-synaptics

Default configuration file: `/usr/share/X11/xorg.conf.d/70-synaptics.conf`  
Copy to `/etc/X11/xorg.conf.d/` and edit here

### Touchpad Natural Scorrling  
In `70-synaptics.conf` file, add following to identifer 'touchpad catchall'
```
Option "VertScrollDelta" "-111"
Option "HorizScrollDelta" "-111"
```
### Touchpad one-click confirm
```
Option "TapButton1" "1"
```

## Battery   
Check battery: acpi

Get: fetch data from specified resources  
Post: submit data to a specified resource


## VScode  
Format code: `Ctrl + Shift + I`

## User Management  
Information about all users: `/etc/passwd`
Information about all groups: `/etc/group`

Each user has a default primary group and some other secondary groups(15 max)


CMD                                | Description
--                                 | --
`id username`                      | get id of user
`useradd username`                 | add user
`passwd username`                  | assign/change passwd to a user
`usermod -u new_id username`       | change id of a user
`usermod -g new_group_id username` | change group of a user
`usermod -l new_name old_name`     | only change login name
`usermod -d new_home_dir username` | change home direcotry of a user
`usermod -r username`              | delete a user
`groups username`                  | get user's group format:(primary secondary)
`usermod -a -G sec_group username` | add a secondary group to user




## Permission  
`ls -l filename`  show permission of a file  

-rw-r--r--  

Part    | Description
--      | -- 
-       | type of file(d for directory, s for special, - for regular file)
rw-     | owner's permission to file
r--     | members of the same group's permission
r--     | all other users' permission





### keyboard

CMD                 | Description
`setxkbmap -print -verbose 10` | see X kb settings


## KDE  

### SDDM
Default configuration file: ```/usr/lib/sddm/sddm.conf.d/default.conf```  
Configuration change: ```/etc/sddm.conf.d/```  
Autologin: ```/etc/sddm.conf.d/autologin.conf```  

## Swap Esp and Caps  
`setxkbmap -option caps:swapescape`. Add this to `.xinitrc`


##  Processes
### Show Processes   
```top```  Display processes and memory use
```up/down``` to navigate  
```k``` to kill  
Attribute | Description  
----------| ----------
VIRT      | Amount of virtual memory
RES       | Amount of physical memory  
SHR       | Amount of memory shared  
S         | State of process
TIME+     | Total CPU time consumed by process

State   | Description
--      | --
D       | uninterruptible sleep
R       | running  
S       | sleeping  
T       | traced or stopped  
Z       | zombie

Process Status command, display current running processes only
CMD     | Description
--      | --
`ps`    | start
`ps -u` | more information
`ps -A` | display all processes

### Kill Processes
CMD                 | Description
--                  | --
`kill (-9)[pid]`    | kill a process. -9 is SIGKILL, by default
`kill -L`           | list all signals
`killall [pname]`     | kill a process


## Vim  

### ColorScheme
`:colorscheme <scheme>`  Change color scheme  
Options:
blue       delek     evening    morning   peachpuff   slate  
darkblue   desert    industry   murphy    ron         torte  
default    elflord   koehler    pablo     shine       zellner  


Cmd                     | Description  
--                      | -- 
`gg`                    | Go to start of file
`G`                     | Go to end of file

## Makefile  
```
targets: prerequisites
    command
    command
    command
```

## picom  
Default configuration file: `/etc/xdg/picom.conf`

## xwallpaper  
`xwallpaper --zoom filename`

## ranger
Copy default configuration to `~/.config/ranger`: `ranger --copy-config=all`  
In `~/.config/ranger`, there are following files

file            | Description  
--              | --
`rc.conf`       | Keybinding and settings
`commands.py`   | commands launced with :
`rifle.conf`    | application used when given type of file is launced
`scope.sh`      | File Preview Settings


#### Console Commands  
Use `space` to select files  
Use `:` followed by commands

CMDs              | Description
--                | --
`:bulkrename`     | Rename in bulk
`:open_with`       | Open a select file with your choosen program
`:touch FILENAME` | create new file
`:mkdir FILENAME` | create new directory
`:shell COMMANDS` | run commands in a shell
`:delete`         | Delete files

Placeholder         | Description  
--                  | --
`%s`                | Currently select file
`%d`                | Current directory

#### Usual Commands

CMDs           | Description
--             | --
`i`            | Preview File
`r`            | Open File
`zh/backspace` | View hidden files
`gh`           | Go to home directory
`g/`           | Go to root directory
`cw`           | Rename File
`yy`           | Copy file
`dd`           | cut file
`dD`           | delete file
`pp`           | paste file
`z + letter`   | Changing settings
`g + letter`   | Go directly to one directory and create/delete/move tab
`c + letter`   |
`y + letter`   |
`d + letter`   |
`p + letter`   | Different paste mode

#### Image Preview  
`set preview_images true`  in `rc.conf` and change `preview_images_method`

#### PDF Preview
Uncomment the following part in "scope.sh"
```
# PDF  
 application/pdf)
    pdftoppm -f 1 -l 1 \

```

## imagemagick
Image viewer in terminal

`display filename`



## MariaDB
`mariadb -u root (-h) -p`
34.66.142.241


## fcitx5


## Git

### Configuration
Path                                                       | Description
--                                                         | --
`/etc/gitconfig``git config --system`                      | config to all users and repos
`~/.gitconfig/ ~/.config/git/config` `git config --global` | config personally to this user and all your repos
`.git/config` in repo `git config --local`                 | specifict to this single repository(default)
`git config --list --show-origin`                          | view all settings
`git config <key>`                                         | check a specific key
`git config --global user.name "sth"`                      |
`git config --global user.email sth`                       | `git config user.name ` to override this setting in a specific repo
`git config --global core.editor vim`                      | set editor

### Useful commands
git commit --amend ????????????????????????????????????????????????

git reset HEAD <file> (old) ??????stage????????????

git clone 		????????????????????????????????????origin/master????????????????????????master?????????????????????????????????????????????

git fetch <remote>	(<branch>) ??????remote/branch????????????????????????????????????merge,????????????branch?????????fetch??????branch

git pull 		 git fetch + git merge??? ?????????????????????????????????(try not to use)
git pull <remote> <branch> ??????remote???branch

git push <remote> <branch> ????????????<branch>?????????remote???????????????????????????<branch>?????????????????????<remote/branch>?????????<remote/branch>)
git push <remote> <localbranch>:<remotebranch> ??????????????????????????????
git push <remote> --delete <branch>  ??????????????????

git remote 	???????????????remote repository
git remote -v	??????url
git remote add <shortname> <url>
git remote rename <old> <new>
git remote remove <name>

git branch   	????????????
git branch -v	??????????????????????????????commit??????
git branch -vv	????????????????????????
git branch <name> ??????????????????????????????commit
git branch -d <name> ????????????
git branch -D <branch> ???????????????????????????
git branch (--merged)(--no-merged) ?????????????????????????????????????????????,??????<name>???????????????????????????
git branch -u <remote/branch>???????????????????????????????????????

git checkout <branch> HEAD?????????????????????t
git checkout -b <branch> ????????????????????????
git checkout -b <branch> <remote/branch> ????????????b??????????????????????????????????????????????????????????????????clone???dev??????origin/dev
git checkout --track <remote/branch> ?????????????????????????????????????????????
git checkout -- <file>  (old)?????????commit????????????????????????????????????????????????

git merge <branch> ???<branch>?????????????????????

git log
git log --oneline (--graph)(--all)

git tag (-l)    ????????????
git tag -a <tagname> -m "" ??????annotated??????
git show <tagname> ???????????????

git tag <tagname> ??????lightweight??????
git tag -a <tagname> <commit number> ????????????????????????

git push <remote> <tagname> ??????????????????
git push <remote> --tags  ??????????????????  
git tag -d <tagname> ???????????????????????????
git push <remote> :refs/tags/<tagname> ?????????????????????  
git push <remote> --delete <tagname> ??????  

git checkout <tagname> ????????????  
git checkout -b <branch> <tagname> ??????????????????

## cat  



CMD                    | Description
--                     | --
`cat filename`         |
`cat -n filename`      | show with line numbers
`cat > newfile`        | create new file
`cat src1 src2 > dest` | copy content from srcfile to dest
`cat src >> dest`      | append file1 to file2
`cat -s filename`      | no repeated empty line


## x86 Assembly  
```
.data                      # readable/writable
data_item:                 # data_items points to 3
.long 3,5,6,7              # declare an array of numbers with 4bytes(consecutive)

.text                      # readable/executable
 
.global _start

_start:                    # entry to instruction
    
```
.quad: 8bytes  
.word: 2bytes

```
.rept 3
.long 0
.endr           # repeat sequence of lines 3 times
```
equivalent to 
```
.long 0
.long 0
.long 0
```

## Sed Command  
Sed stands for stream editor. Used for inserting, deleting, searching or replacing sth in file and print the result to stdout. It WON'T modify the original file

Commands                    | Description  
--                          | --  
sed 's/unix/linux/' FILE    | Replace unix with linux in the file, only replace the first occurence of the pattern in each line
sed 's/unix/linux/2' FILE   | Replace the second occurence of unix in the file
sed 's/unix/linux/g' FILE   | g:Global. Replace all occurences  
sed 's/unix/linux/3g' FILE  | Replace from 3th occurence to all occurences **in a line**
echo "have a nice day" | sed -E 's/([a-z])/\(\1\)/g' | Add parenthesis to every letter. -E to enable extended regular expression so that () doesn't need backslash before it. \1 to represent the matched pattern
sed '3 s/unix/linux/' FILE  | Replace only the third line
sed '1,3 s/unix/linux/' FILE| Replace from the first line to the third line
sed '2,$ s/unix/linux/' FILE| $ means last line, replace from second line to the last line
sed '5d' FILE               | Delete 5th line of the file
sed '3,6d' FILE             | Delete 3rd to 6th line of the file
sed '/abc/d' FILE           | Delete pattern matching line
sed 'G' FILE                | Insert a blank line after each line
sed '/love/G' FILE          | Insert a blank line after each line with matching pattern love


s: substitution  
/: delimiter


## Awk Command

awk is used for manipulating data.  
`awk [options] 'selection criteria' <input file>`

Note:  
Record: Each record is separated by a record separator, by default the separator is newline
Field: Each field is separated by a field separator, by default the separator is whitespace


Built-in Variables  | Description  
--                  | --  
NR                  | current number of input records/lines
NF                  | current number of fields(separate by whitespace) in current line
FS                  | Current Field Separator, by default whitespace
RS                  | current Record Separator, by default newline
OFS                 | Output Field Separator, by default whitespace, when print several parameters separator with commas, it prints value of OFS between each parameter
ORS                 | Output Record Separator, by default newline. print ORS at the end of whatever it is given to print

Command                               | Description
--                                    | --
awk '{print}' FILE                    | Print the content of the file
awk '/manager/ {print}' FILE          | Print all lines matching the pattern 'manager'
awk '{print $1,$4}' FILE              | Split each line based on whitespace and print the 1st and 4th field
awk '{print NR,$0}' FILE              | Display line number, $0 stands for all contents of current record
awk 'NR==3, NR==6 {print $0}' FILE    | Display from 3rd line to 6th line
awk '{print $1 "-" $2}'               | Field 1 and 2 are separated by "-"
awk 'END {print NR}' FILE             | Count the number of lines in the file
awk 'length($0) > 10' FILE            | Print lines with length larger than 10
awk '{if($3 == "B6") print $0;}' FILE | Find string in specific column

## Regular Expression

Example 1:
```
$echo 'abc' | sed 's/./xyz/g'
xyzxyzxyz  
```
Here a dot means **any character**  

Example 2:  
```
$echo 'abc' | sed 's/\./xyz/g'
abc
```
Use back slash to avoid interrept dot as regular expression

Example 3:  
```
$echo 'a..b..c' | sed 's/[\.b]/d/g'
adddddc
$echo 'a..b..c' | sed 's/[\.b]\+/d/g'
adc
$echo 'a..b ..c' | sed 's/[\.b]\+/d/g'
ad dc
$ echo 'a..b..c' | sed 's|[\.b]\+|d|g; s|[a-c]|d|g'
ddd
```
`[\.b]`: Any literal dot or character b  
`\+`: Look for at least one, possibly more of listed characters and the matched pattern will be considered as only a single occurence. Here `\+` is not a literal `+`.  
`[a-c]`: A range of letters from a to c  


Example 4:
```
$echo "have a nice day" | sed 's/$/ you all/'
have a nice day you all
```
`$`: means end of line

Example 5:  
```
$echo "sample+" | sed 's/[a-e]+/_/g'
sampl_  # Here + is just a +
$echo "sample+" | sed -E 's/[a-e]+/_/g'
s_mpl_+ # Here + is not a +
```
-E: Enable Extended Regex.  In extended regex, + just means /+

Example 6:
```
$echo "abcdefghijklmnopqrstuvwxyz ABCDEFG 0123456789" | sed -E 's/([a-o]+).*([A-Z]+)/\2 \1/'  
G abcdefghijklmno 0123456789
```
`.*`:Any character, 0 or more times.It starts matching after character o and it will keep matching characters until the last A-Z are matched. This part is not captured by sed
`\2 \1`: Change the order of lower-case letters(first search pattern) and upper-case letters( second search pattern )

Example 7 (Easier version of Example 6):  
```
$echo "have a nice day " | sed 's/e.*e//g'
hav day
$echo "have a nice day " | sed 's/e.*//g'
hav
$echo "have a nice day " | sed 's/.*e//g'
 day
$echo "have a nice day " | sed 's/.*[a-d]//g'
y
$echo "have a nice day " | sed 's/e.*[a-d]//g'
havy 
```
Example 8:  
```
$echo "abcdefghijklmnopqrstuvwxyz ABCDEFG 0123456789" | sed -E 's/[^ ]*/_/'
_ ABCDEFG 0123456789
```
`[^ ]*`: Match any non-space character, 0 or more times. ^ in [] means not

Example 9:
```
$ echo "have a nice day" | sed -E 's/[^ ]+ [^ ]+//'
 nice day
```
`[^ ]+ [^ ]+` means the pattern of no-space space no-space  

**Summary**

Metacharacters | Example         | Description
--             | --              | --
[]             | [a-m],[0-9AF-Z] | A set of characters
[^ ]           | [^A-Za-z]       | Outside the selected range
.              | he..o           | Any character
*              | he.*o*          | Zero or more occurences, usually followed by other metacharacters like .*
+              | he.+o           | One or more occurences
?              | he.?o           | Zero or one occurences
{}             | he.{2}o         | Exactly specified number of occurences
^              | ^hello          | Start of the string
$              | planet$         | End of the string
`|`            | `fall|stays`    | Either or
\d             |                 | One digit
\D             |                 | One non-digit
\s             |                 | One whitespace
\S             |                 | One non-whitespace
\b             |                 | Backspace character
\n             |                 | Newline character
\t             |                 | Tab character










## Bash

### Commands  
Every part separated is called **word** 
Arguments are separated by blankspace with command, `[]` is also a command'
`[-f file]` (wrong)   
`[ -f file ]` (correct)  

In Bash, almost everything is string (command, argumetns etc)

### Types of commands  
**Alias**: A way of shortening a cmd, only in interactive shell  
**Functions**  
**Builtins**: builtin commands 
**Keywords**: part of Bash syntax
**Executable**  

### Empty Space
Use double quote if there's whitespace belongs to one word
`var=a b` is wrong and `var="a b"` is correct


### Special Characters  
Character | Description
--        | --
`[[  ]]`  | *Test* - evaluation of a conditional expression
`{}`      | *Inline group* - cmds inside are treated as one cmd
`()`      | *Subshell group* - cmds insde are executed in a subshell
`((  ))`  | *Arithmetic expression* - +,-,* inside are treated as operator
`$((  ))` | the expression will be replaced with the reuslt of arithmetic evaluation  
`\`       | *Escape* - Prevents next character from being interpreted as a special character.

### Quotation

Double quotes can be used for strings containing spaces. Some times, it's not necessary to wrap a variable with double quotation like `"$var"`,but it is always a good idea to do so.  
Single quote will cause Bash to interrept string literally  
Back quote allow us to execute command  

```
echo $var         - > hello world
echo "$var"       - > hello world (a better habit)
echo '$var'       - > $var
echo '"'"$var"'"' - > "hello world" (put single quote around double quote so that it is interrrepted literally)
echo \""$var"\"   - > "hello world" (escape double quote so that it is printed)
```

### Parameter Expansion  

`$var` variable expansion
`${var}s` Seperate s from the variable
`$(command)`  command expansion  
`$((expression))`  arithmetic expansion
```
$ echo "'$USER', '$USERs','${USER}s'"
'autentico', '', 'autenticos'
```
### String Concatenation  

Example                     | Description
--                          | --
`var=$var1$var2`            | Concatenate two variables

Command                     | Description  
--                          | --  
`${#parameter}`             | length of parameter in character, if parameter is array, it returns the length of array  
`${parameter#pattern}`      | delete pattern from start, match against start, short
`${parameter##pattern}`     | delete pattern from start, use longest match from start
`${parameter%pattern}`      | delete pattern from end, match against end, short
`${parameter%%pattern}`     | delete pattern from end, use longest match from end
`${parameter/pat/string}`   | replace first pat with string, string can be null
`${parameter//pat/string}`  | replace every pat with string
`${parameter/#pat/string}`  | match against beginning, useful to add prefix
`${parameter/%pat/string}`  | match against end, useful to add suffix

```
$var=1.5.9
${var#*.}   -> 5.9
${var##*.}  -> 9
```

### Substring  
`${VAR:start_index:length}   # start_index from 0`  
`${MYSTR:11:4}`


### Command Substitution
Insert the output of one command into a second command  
Example: `cur_time=$(date)`  


### Parameters
Parameters include *variables* and *special parameters*  
variable: parameters that are denoted by a name  
special parameters: not denoted by a name  

Assign a variable  
`varname=data`  
Access data using *parameter expansion*   
`echo "$varname"`



Special Parameters | Description
--                 | --
`0`                | name/path of the script
`1,2,3`            | *Positional Parameters* arguments passed to current script
`*`                | Expands to all words to all positional parameters. If double quoted, it expands a string containing them all
`@`                | Expands to all words to all positional parameters. If doube quoted, it expands to a list containing them all
`#`                | Number of postional parameters currently set
`?`                |
`$`                | PID of current shell
`!`                | PID of command most recently executed
`_`                | last argument of last cmd executed

To expand these parameters, add $ before them

Predefined Variables            | Description  
--                              | --  
BASH_VERSIOn                    |
HOSTNAME                        |
PPID                            | PID of parent process of shell
PWD                             | current working directory
RANDOM                          |
UID                             | ID of current user
COLUMNS                         | width of terminal in characters
LINES                           | height of terminal in characters
HOME                            |
PATH                            |



### Variable Types
Command               | Type
--                    | --
`declare -a/array=()` | Array
`declare -A`          | Associative array
`declare -i `         | Integer, rarely used
`declare -r`          | Read Only



### Glob Patterns
Glob=normal character + metacharacter

Metacharacter               | Description  
--                          | --  
*                           | Match any string
?                           | Match any single character
[...]                       | Match any one of enclosed character

### Extended Glob  
Enable extended glob using `shopt -s extglob`

Metacharacter   | Description  
--              | -- 
?(list)         | Match zero or one occurence of pattern
*(list)         | Match zero or more occurence of pattern  
+(list)         | Match one or more occurence of pattern 
@(list)         | Match one of given patterns
!(list)         | Match anything but given patterns

list: list of (extended) globs separated by `|`  
```
$ ls
names.txt  tokyo.jpg  california.bmp
$ echo !(*jpg|*bmp)
names.txt
```

### Brace Expansion
```
$ echo th{e,a}n
then than
$ echo {/home/*,/root}/.*profile
/home/axxo/.bash_profile /home/lhunath/.profile /root/.bash_profile /root/.profile
$ echo {1..9}
1 2 3 4 5 6 7 8 9
$ echo {0,1}{0..9}
00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19
```

### Exit Status  
Each command has an exit status. Range 0-255, 0 means success. `$?` to get exit code of last command


### Control Operator (&& || )
`&&`: If the first command return exit code 0, it will execute the next command.  
`||`: If the first command doesn't return exit code 0, it will execute the next command. Useful for simple error handling
`!`: Negate

### Grouping Statements
Use `{}` to group statements and the group is considered as one statement instead of several.  
Example. Without grouping, which will execute `echo` even though the first command fails:
```
$ grep -q goodword "$file" && ! grep -q badword "$file" && rm "$file" || echo "Couldn't delete: $file" >&2
```  
With grouping, which is correct:
```
$ grep -q goodword "$file" && ! grep -q badword "$file" && { rm "$file" || echo "Couldn't delete: $file" >&2; }
```

### Conditional Blocks
`if` will check the exit code of `COMMAND1`. If it's 0, it will execute the `then` part  
```
if COMMAND1
then COMMAND2
elif COMMAND3
then COMMAND4
else COMMAND5
fi
```

Test command:`[]` or 'test' and `[[]]`, test things and return an exit status 
`[ a = b ]` 
`[[]]` allows pattern matching while `[]` doesn't  
`= != < > ` treat their arguments as strings   
`-eq -ne -lt -gt -le -ge` treat arguments as numbers

Tests           | Description
--              | --
-e FILE         | if file exists
-f FILE         | if is regular file
-d FILE         | if is directory
-s FILE         | if file exists and not empty
-r FILE         | if file is readable
-w FILE         | if file is writable
-x FILE         | if file is executable
FILE1 -nt FILE2 | if file1 is newer than file2
FILE1 -ot FILE2 | if file1 is older than file2
-z STRING       | if string is empty
-n STRING       | if string is not empty

Tests support by `[[]]`
Tests             | Description
--                | --
STRING = PATTERN  | if string matches the glob pattern
STRING == PATTERN | same as above
STRING != PATTERN | if string doesn't match the glob pattern
STRING =~ REGEX   | if string matches the regex


### Conditional Loops
```
while COMMAND
do COMMAND
done

for ((i=10; i > 0; i--))
do COMMAND
done

for i in {10..1}
do COMMAND
done
```

### Choices  
```
case $LANG in 
    en*) COMMAND ;;
    fr*) COMMAND ;;
    de*) COMMAND ;;
    *) COMMAND ;;
esac
```

If use `;&`, the block of next code will be executed no matter if pattern for that choice matches or not  
If use `;;*`, the block of next code will be executed depending on if the condition satisfies

```
select choice in A B C
do
echo "Your choice $choice"
done
```

### Arrays  

#### Declare an array:  
`names=("Bob" "Peter" "Big John")`  
`names=([0]="Bob" [1]="Peter" [20]="Big John")`  
`names[0]="Bob"`  
`unset 'names[1]' #remove an element to make in sparse`
`array=("$(array[@])")  # Reindex to remove holes in sparse array`
`photos=(~/pictures/*.jpg)`  
`file=(*)`

In order to parse stream(e.g. Output of a command) into elements of array, we can use NUL byte to indicate where each element starts and ends. As output of a command often can make its output separated by NUL.

#### Print a (array) variable
```
$declare -p names
declare -a names=([0]="Bob,Peter,Big John")
```

```
echo "${file[@]}"
echo "${file[0]}"
echo "${#file[@]}"  # Length of an array
```

```
printf '%s\n' "${file[@]}"   # $@: Expands to a list of words, with each array element as one word. 
```

```
for file in "${file[@]}"
do COMMAND
done
```

Remember always to quote properly!  
```
$for name in ${names[@]}; do echo $name; done
Bob
Peter
Big
John

$for name in "${names[@]}"; do echo $name; done
Bob
Peter
Big John

$for name in ${names[*]}; do echo $name; done
Bob
Peter
Big
John

$for name in "${names[*]}"; do echo $name; done
Bob Peter Big John  # If you use *, it will be a string

```


#### Expanding Indices  
```
$echo "${!names[@]}"
0 1 2       # In this way, you get the index of an array and can possibly use a for loop iterating indices.
```

#### Associative Array
```
declare -A fullNames    # declare an associate array
fullNames=( ["Bob"]="Bobbe Lorry" ["Peter"]="Pettery Parker" )
declare -p fullNames
echo "${fullNames['Bob']}"
```
Context in [...] in a indexed array is arithmetic, you can do math here without wrapping using $((...)).  
For indexed array, index in [...] part doesn't need a dollar sign$, but for associated array, an dollar sign is required. 
```
$ indexedArray=( "one" "two" )

$ declare -A associativeArray=( ["foo"]="bar" ["alpha"]="omega" )

$ index=0 key="foo"

$ echo "${indexedArray[$index]}"

one

$ echo "${indexedArray[index]}"

one

$ echo "${indexedArray[index + 1]}"

two

$ echo "${associativeArray[$key]}"

bar

$ echo "${associativeArray[key]}"

$ echo "${associativeArray[key + 1]}"

```



### Command Line Arguments
```
myfile arg1 arg2 arg3  
$0     $1   $2   $3   # $@ is set of all positional parameters
```
If you `shift` $1, $1 goes away and $2 becomes $1, $3 becomes $2 and so on

### File Descriptors  
Reference to files and other resources like files.
By default, there are three FDs:  
    + stdin, FD 0, from keyboard
    + stdout, FD 1, display on monitor
    + stderr, FD 2, display on monitor

`read`: read info from *stdin* and store in a variable  
`-p`: print according message
`read -p "What's your name?" name_variable`

### Redirection  
Redirection works by changing what FD points to. For example, change FD1 points to stdout to regular file.  

Redirection         | Description  
--                  | --  
`>`                 | Output redirection, stdout will write to a file 
`<`                 | Input redirection , stdin will read from a file 
`>>`                |

By default, if you simply type `cat`, it will read input from `stdin` and print to `stdout`. You can do some changes using redirection  

`cat < story`

Use number to denote FD that will be changed
`command > file`  
`command 1> file`
`command < file`
`command 0< file`

`rm file_that_not_exist 2> errors  # store error log in a file`
```
for sth in sths
do COMMAND
done 2> errors      # redirection applies to all output to stderr inside the loop
```

Ignore error message.  
`COMMAND 2> /dev/null`




### File Descriptor Manipulation







### grep
Reads the files and searches for the pattern provided.

`grep PATTERN FILEs` (from stdin if no file indicated)  
`grep -r `     # supports directories, it will search all files in this directories


In the following command, two indenpendent FDs point to the same file.
The second FD points to beginning of `proud.log` and it will overwrite the information.  
`grep proud file not_file > proud.log 2> proud.log`  
Result in proud.log:  
```
grep: not_file: No such file or directory
 not and this is really amazing.
```


In the following command,`>&1` duplicate FD1 to FD2. Write to these FDs are the same and there's no mix-up.  
`grep proud file not_file > proud.log 2>&1`
Result in proud.log:  
```
file:I am proud whatever you believe it or not and this is really amazing.
grep: not_file: No such file or directory
```

### Heredoc and Herestring

Embed short blocks of multi-line data in to the command  
Example of heredoc:
```
cat << CONTENT  
>This is
>Really
>Amazing
CONTENT
```

Example of herestring:
`stdin` read information from string after `<<<`
`grep proud <<< "I am really proud"` 



### FIFO  
`mkfifo FILENAME`  
Create a special file, first write, first read, read will block until write to the file. Write will block until another process read FIFO.  


### Pipe  
Connects `stdout` of one process to `stdin` of another.  
Pipe operator will create a subshell environment for each command, so variables you modify inside the second command will appear unmodified outside of it.    


### Process Substition  
`>(command)` write  
`<(command)` read, run the command inside the parentheses and give a temporary filename that you can use.
Example   
`diff <(sort file1) <(sort file2)`


### Subshell  
Create subshell explicitly using parentheses `()`  
Any variables set during subshell are not remembered  
`(cd /tmp || exit 1; date > timestamp)`  
If `cd` fails, `exit 1` will terminate the subshell instead of the interactive shell.

### Command Grouping  
use `{}` to group commands.Separate commands with semicolon and there must be a semicolon after the last command. Or you can write commands in multiple lines. Basically it has no effect. But it allows a collection of commands to be considered as a whole with regard to redirection and control flow.`if, for, while` are already compound commands which behave like command grouping.  
If you use redirection to a command group, the file FD point to will only open once and close once, instead of opening the file and closing the file for each command in the group.


### Arithmatic Evaluation  
```
$a=2+3         # a is '2+3'
$let a=2+3     # a is 5
$let a='2 + 3' # a is 5
$((a=2+3))     # a is 5, which is arithmatic evaluation 
$echo $((a*b)) # arithmatic substitution  
```
`((abs = (a>0) ? a : -a))`  
`if ((flag)); then COMMAND; fi      # note $ sign is not required`  


### Functions
```
sum(){
    echo "$1 + $2 = $(($1 + $2))"
}
$sum 1 3
1 + 3 = 4
```
```
#!/bin/bash
count() {
    local i         # Local variable in the function
    for ((i=1; i<=$1; i++)); do echo $i; done
    echo 'Ah, ah, ah!'
}
for ((i=1; i<=3; i++)); do count $i; done
```

To remove a variable, alias or a function:  
`unset -f myfunction`  
`unset -v myvariable`
`unalias myalias`

### Sourcing  
When a script finishes its execution, its environment is discarded.  
`. ./myscript`  
If you use the dot operation, the script will run in current shell's environment. So Current shell's variables, working directory, functions etc will be changed.










