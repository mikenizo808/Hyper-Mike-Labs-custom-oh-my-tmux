# Terminal Experience

Here we install fonts (which are optional), and then we install `tmux` and `oh my tmux`.


## Optionally, install `powerline` fonts, etc.

Although we do not need `powerline` fonts to use `oh my tmux`, I still install these anyway.


    sudo apt install powerline fonts-powerline powerline-doc python3-i3ipc vim-addon-manager -y


> Note: The `oh my tmux` documentation and configuration comments will remind you that you do not need `powerline` fonts.


## Optionally add the Adobe font

This is another optional font set. This one is from `Adobe` and looks good on `tmux`.
When you download the `zip`, there will be a handful of fonts to choose from.
Once extracted with `unzip` or similar, double-click to install one or more
fonts such as `Source Code Pro Medium`.

See the official github at [https://github.com/adobe-fonts/source-code-pro/releases/tag/2.042R-u/1.062R-i/1.026R-vf](https://github.com/adobe-fonts/source-code-pro/releases/tag/2.042R-u/1.062R-i/1.026R-vf). Here we download the latest at the time of this writing.

    cd ~/Downloads
    mkdir AdobeFonts
    cd AdobeFonts
    wget https://github.com/adobe-fonts/source-code-pro/releases/download/2.042R-u%2F1.062R-i%2F1.026R-vf/OTF-source-code-pro-2.042R-u_1.062R-i.zip
    unzip ./OTF-source-code-pro-2.042R-u_1.062R-i.zip

    ## manual steps
    - Open a file browser and preview a font by double-clicking it
    - You will have an option to install the font at that time
    - Later, you can review your fonts with the "fonts" application in your Ubuntu `Applications > Tools` menu
    - Recommend finsihing the `tmux` section below to see your default fonts before adding custom fonts to your terminal


## About `tmux`

The `tmux` package is a terminal multiplexor that allows you to split your terminal window
vertically, horizontally, and so much more.

We will install and configure `tmux` and use it as our base upon which to install `oh my tmux` later.


## Check if you have `tmux` yet

    apt list tmux


## Install tmux

    sudo apt install tmux -y


## Using `tmux`

You can start a session by typing `tmux` from a terminal, and hitting enter.
Then you can access the `tmux` quick shortcut commands with `CTRL+b` which
then waits for your command such as `"` or `%` to split the screen. To
close any extra screens you are done with, type `exit`.

If it has been a while, you will pick it back up! To see the available commands,
press `CTRL+b` and then `?`.


## Check if you have a tmux profile yet

This probably will not exist yet.

    ls -lh ~/.tmux.config


## Create a `tmux` profile, if needed

This section is intended for a fresh `tmux` installation with no configuration yet.

Ignore this step if you already have `oh my tmux` running.

Here, you can do a `touch ~/.tmux.conf`, or simply use
`vi` to create and edit the file. By using `vi` the
system will open the file if it exists or create
it if it does not exist yet.

    vi ~/.tmux.conf


# Overview of what is about to happen in the next steps

In the next steps we will get `oh my tmux` running which itself
does some magic to use any existing `tmux` configurations such as `~/.tmux.config`
and then it creates a `~/.tmux.config.local`, which is what is actually used at runtime by `oh my tmux`.

It is worth mentioning that sometimes we use that shortpath `~/.tmux.config` when wanting to reload the conifg.
We do that in a bit.


## Optionally add some entries to the default `tmux` config

These will vanish when you go to `oh my tmux` so back them up if needed
Add these options to the file `~/.tmux.conf`
These are just some examples, see `man tmux` for more details.

Once you go to `oh my tmux` these will be gone.

    #optionally edit the tmux config or create one now
    vi ~/.tmux.conf

    #add some entries
    set-option -g status-style bg=lightblue
    set-option -g lock-after-time 1800

    #press :wq! to write to the file and exit.


## Reload the `tmux` profile

    #update tmux runtime settings
    tmux source-file ~/.tmux.conf


## Look at your `tmux` setup so far

Launch a new terminal and type `tmux`.  To exit and go back to your terminal type `exit`.


## Choose a location and then install `oh my tmux`

Official instructions have more options for locations to set things up.

We will use the home location of `~` from the official examples.
Do not make up your own folders here, follow this exactly or
see the vendor instructions.

    cd ~
    git clone --single-branch https://github.com/gpakosz/.tmux.git
    ln -s -f .tmux/.tmux.conf
    cp .tmux/.tmux.conf.local .


## Example file listing with `ls`

We can see that `oh my tmux` creates a symbolic link from `~/.tmux.conf` to `~/.tmux.conf.local`.

    mike@ubuntu-laptop:~$ ls -lh ~/.tmux*
    lrwxrwxrwx 1 mike mike   16 Jan 10 17:15 /home/mike/.tmux.conf -> .tmux/.tmux.conf
    -rw-rw-r-- 1 mike mike  25K Jan 12 12:03 /home/mike/.tmux.conf.local


## Reload the `tmux` profile (again)

Even though we have `oh my tmux` installed now, the syntax is the same.

You can come back to this over time.

    #refresh tmux after making changes to ~/.tmux.conf.local
    tmux source-file ~/.tmux.conf


> Note: Even though our editable configuration is now at `~/.tmux.conf.local`, we run the `tmux source-file` command with `~/.tmux.conf`. 


## If everything is working

Simply running tmux should show the fancy `oh my tmux` bar at the bottom.

    tmux


## Backup the default configuration

Optionally make a backup of the `oh my tmux` default configuration

    cp ~/.tmux.conf.local ~/Documents/backup-of-default-oh-my-tmux.txt


## Missing the fancy bar at the bottom

If you are not getting the powerline status bar, exit `tmux` by typing `exit`,
and then run `tmux source-file ~/.tmux.conf` again.

> Official Link about this tip at [https://github.com/tmux-plugins/tmux-cpu](https://github.com/tmux-plugins/tmux-cpu)


## Optional Plugins

`oh my tmux` has special handling for `tmux` plugins, if used.
For example, it will add the required call to load and unload the plugins.
To learn about the available plugins see [https://github.com/tmux-plugins](https://github.com/tmux-plugins).
However, do not add any custom code as shown on the plugin website since this is all handled for you by `oh my tmux`.

For example, the plugin website asks you to load and un-load the plugins manually, but `oh my tmux` already has
this setup where you can simply un-comment the approriate lines in the `/tmux.conf.local` file to use the
desired plugin(s).

## Optional `tmux` session handling
If you just booted your system you can type `tmux` and this will start the server
and client connection for `tmux`. This is all local to your system, but this is
how it works.

There is one "server" process that runs and handles one or more `tmux` sessions.
If you did not close your session properly you might have a `tmux` session
in the background, ready to be used with `tmux attach`.

    tmux list-sessions
    tmux list-windows
    tmux attach

> Note: You can suspend a `tmux` session with `ctrl+b d` (or `tmux detach`), then later attach again. 


## Optional - Set a default font for your terminal

Now that `oh my tmux` is setup, this is a good time to see if you like other fonts.
Optionally, add one of the fonts we downloaded earlier such as `Source Code Pro Medium` as your terminal default.

You can typically do this by launching a terminal and going to preferences. Make note of what the default is in case you want to go back.


