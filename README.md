# Hyper-Mike-Labs-custom-oh-my-tmux
My custom tmux configuration including people in space (requires PowerShell 7 or better). Adjust to point to the desired city for weather (currently Chicago). 

This setup runs great on `Ubuntu 24.04` which is my native setup. For more tips on setting up your terminal or getting started with `tmux` see the included guides in this repository. They are a sneek peek at my upcoming full guide showing how to setup your laptop for develeopment on Linux (specifically `Ubuntu 24.04`, but use your favorite!).

Click on releases to the right to get actual configuration file. Everything else is just documentation.

## Steps

- First, read the "Termina Experience" markdown file. This will get you setup with tmux and oh my tmux.
- Then, you can optionally review the "Advanced Terminals" guide to get setup with `ghostty`, etc.  Be sure to check for updates as my versions of `ghostty` listed may be aging.
- Finally backup your existing `tmux.conf.local`, if needed and then copy/replace with the custom configuration available from the releases on the right

> Note: The file has a .txt extension but you will rename it to `tmux.conf.local` which goes right in your `~`, a.k.a. home directory.  You can `ls -lh ~/.tmux.conf.local` to see the file.  To edit with your code editor browse to your home directory and then press `SHIFT-h` which will show/hide hidden files (on `Ubuntu` anyway).

Then you can just run `tmux`.  To keep your existing session you can `tmux detach`. To come back `tmux attach` or `tmux list-sessions`.  See the included guides for more tips like reloading the configuration, etc.
