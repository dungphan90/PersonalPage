## What is `tmux`?

`tmux` is short for **t**erminal **mu**ltiple**x**er. It allows user to have multiple terminal sessions to be simulteneously accessible in a single window. So you might think that it's basically just like having multiple tab on a terminal emulator like `iterm2` or `gnome-terminal`. What is the benefit?

`tmux` is not like having multiple tabs on a terminal emulator. When you kill a terminal emulator, the session dies with it. `tmux`, on the other hand, allows the session, along with running processes, to be detached from the console. I will give some examples of this below, but before going into details, I will go quickly over what are the benefits of using `tmux` and in which case I would reccommend you to use `tmux`.


## When `tmux` becomes useful?

### Working by `ssh` to remote server

Let say your work involve connecting to a remote server via `ssh`. Without `tmux`, if for whatever reason (disconnection, your local terminal crash, ...) the `ssh` session is killed, any processes that is running on the server will be terminated. If you once experienced this issue, you knew how annoying and distracting it is to your work.

With `tmux`, the remote terminal session will stay even if the `ssh` connection is no longer running. This is actually one of many reasons I started to look into `tmux`. 

### Reducing Setting-up Overhead
There is always some overhead in starting up your work environment. In my case, I usually need to log into a specific machine on a remote server, calling a few scripts to setup a development that has been trimmed to my needs, then `cd`ing to a specific code base folder, editing some files, having a few separate terminals with a tiny different environment for building and running the code, etc. So, setting up a work environment for me would take a minute or so everytime. Worst, there are cases when I need to work with multiple files and I couldn't remember to open them all. Open a new terminal and locate the file that I need mid working session is really distracting for me.

`tmux` is the perfect answer for this issue. All you need is creating a named `tmux` session with all your windows, panes, and setup up and running for once. After a working session, you will simply detach the session. All your processes that were running in the `tmux` session will still be there, waiting you to re-attach to continue the work. I usually have a few sessions that are few weeks old, always ready for me with all the opening files and `vim` cursors placed right where my past edition is being paused. Saving even the smallest overhead will have a strong effect on your productivity on the long term.

`tmux` sessions allow me to quickly switching between workflows and/or projects. For example, with a few key strokes, I can really move from C++ coding to a front-end web development work. There are no overhead of starting or switching between workflows at all.

### Screen Real Estate Usage
As mentioned briefly in the previous paragraph, in a coding session, I need multiple terminals for code viewing/editing, compiling, running, version controlling, etc. Having many terminal emulator windows opening, to me, is not ideal though. I also tried using a couple of tiling window managers but none actually fit my needs and I feel like not all screen real estate is well utilized. 

`tmux`, again, is the perfect solution to me due to its simple usages of panes and windows. The pane seperator in `tmux` is a single-pixel line and all the screen is utilized to its most potential. The key strokes to move between panes/windows are not as great as those of `emacs` but, at the same time, not terrible. 

## Using `tmux`

### Installation
Installing `tmux` is simply as
```bash
sudo apt-get install tmux #on Ubuntu or
sudo pacman -S tmux #on ArchLinux or
bew install tmux #on MacOs
```
### Session
`tmux` session can be attached or detached at any point of its lifetime. All the processes running in a session will keep running no matter a terminal emulator is attached to the session or not. So our basic usage of a session will be: creating a named session, working on a project in that session, detaching from the session if we want a break, re-attaching if we want to continue working and only killing the session when the work is absolutely done.

My productivity trick is having one session for one specific task (like implementing a new feature or debugging an issue). I will keep the session alive as long as I need (usually few weeks old). And I give each session a easy-to-remember name so I can quickly re-attach to it.

Commands to control a session are listed below.
1. To create a session
```bash
tmux new # Untittle session
tmux new session ## Untittle session
tmux new -s <SessionName> ## Create a new session name "SessionName"
```
If you working with a remote server via `ssh`, the session should be initiated on the remote machine, not your local machine. So you will need to `ssh` on a normal, non-`tmux` session from your computer to the remote machine. Once on the remote, create a `tmux` session there. Make sure to have `tmux` client on the remote machine. If that's not the case, contact the server admin to install it for you.

2. To kill a session. If you `exit` from all the `$SHELL` in a `tmux` session, the session will be killed. To immediately kill one or multiple sessions, you can also use the following commands.
```bash
tmux kill-session -t <SessionName> ## Kill a specific session
tmux kill-session -a ## Kill all sessions except the current one
tmux kill-session -a -t <SessionName> ## Kill all sessions except a specific one
```

3. Detach and re-attaching a session. When we detach from a session, that session is still running in the background. So detaching is just like you pausing the work you are doing. You can always come back to where you left off by re-attaching.

```bash
tmux detach
tmux attach ## Attach to the session, if there is only one session online
tmux attach-session -t <SessionName> ## Specify the session you want to attach to
```

4. Moving between sessions. Eventhough this is possible, as I mention earlier, I would like to organize the working session so that one terminal client tab or window is handling one and only one project. This means I don't need to move between sessions. But if you do, the key strokes are `Ctrl+b (` to move to previous session and `Ctrl+b )` to move to next session.

### Key Strokes in `tmux`
Key strokes in `tmux` always starts with `Ctrl+b` following by a single character. Below is the most frequently used key strokes to control panes and windows. 

1. Panes
```
Ctrl+b %                  --- Split pane vertically
Ctrl+b "                  --- Split pane horizontally
Ctrl+b <arrow-keys>       --- Move to a different pane
Ctrl+b Ctrl+<arrow-keys>  --- Resize a pane
Ctrl+b x                  --- Close a pane (you can simply exit from the session in the pane)
```

2. Windows
```
Ctrl+b c                  --- Creating a new window
Ctrl+b p                  --- Go to previous window
Ctrl+b n                  --- Go to next window
```

3. Copy mode
In the normal `tmux` mode, you cannot use the mouse scroll or arrow keys to move a text buffer. You need to enter copy mode to do that.
```
<arrow-keys>              --- Navigate in copy mode
h-j-k-l                   --- Navigate in copy mode
Ctrl+Spacebar             --- Start selection
Esc                       --- End selection
Ctrl+w                    --- Copy selection
q                         --- Quit copy mode
```

## Tips and Tricks
1. These are few tips to assist you in using `tmux`. First is using aliases. I keep these aliases in my `.zprofile`.
```bash
alias tms="tmux new -s"
alias tmk="tmux kill-session -t"
alias tma="tmux attach-session -t"
alias tml="tmux ls"
alias tmd="tmux detach"
```

2. The resizing can be done by holding all three keys `Ctrl`, `b` and a `<arrow-key>`.

3. I keep a window dedicated for a `vim` session that works as a journal to remind me of progress of the project in the current session, or things that I left off from the last working session. 

4. [A little demo](https://www.youtube.com/watch?v=jdp55ixy7O4)

5. My `~/.tmux.conf`

```bash
# Using color in tmux
set -g default-terminal "screen-256color"

# New panes and windows open in current directory
bind c new-window -c "#{pane_current_path}"
bind '"' split-window -c "#{pane_current_path}"
bind % split-window -h -c "#{pane_current_path}"
```
