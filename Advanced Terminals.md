
# Advanced Terminals

Here we install the `ghostty` terminal, which is a new light-weight package
available for download on most architectures. Since this also compiles very
easily (using `zig`), we will compile it ourselves.

No experience is required. Follow along to install `zig` and compile your very own `ghostty` terminal.

## Objective
Download `zig` (version `0.13`) and `ghostty` source, then compile `ghostty`

## Get zig v0.13

    cd ~/Downloads
    wget https://ziglang.org/download/0.13.0/zig-linux-x86_64-0.13.0.tar.xz
    tar -xvf ./zig-linux-x86_64-0.13.0.tar.xz

# Do not skip this step, cd to zig 13 directory

    cd zig-linux-x86_64-0.13.0

## Optionally show `zig` version
By using `./` we call the `zig` binary in the current folder location.

    ./zig version

## Download ghostty source

    #old
    #wget https://github.com/ghostty-org/ghostty/archive/refs/tags/v1.0.1.tar.gz
    #tar -xzvf ./v1.0.1.tar.gz

    #new 1.1.2
    wget https://github.com/ghostty-org/ghostty/archive/refs/tags/v1.1.2.tar.gz
    tar -xzvf v1.1.2.tar.gz



> Note: Once extracted, `v1.0.1.tar.gz` will become a folder called `ghostty-1.0.1`.

## Navigate into the `ghostty` directory

From this directory, though we can compile.

    cd ./ghostty-1.0.1


## Confirm you see still see `ghostty` version `0.13`
By using `../` this time, we call the `zig` binary that is located
two directories above the current location.

    ../zig version


## Install ghostty build dependencies

    sudo apt install libgtk-4-dev libadwaita-1-dev git


## Compile exactly like this

    ../zig build -p $HOME/.local -Doptimize=ReleaseFast


## log off or reboot

You might need to log off or reboot.
When you login again, `ghostty` will be ready.

## Find the `ghostty` application icon

You should now see the `ghostty` icon in your Ubuntu application menu.
Optionally, right-click the icon and pin to dashboard.



