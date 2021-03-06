# neuFlow

**neuFlow** is dataflow architecture optimized for large array/tensor
transforms, and especially image processing operations.  More info about the
architecture, hardware and applications can be found
[here](http://www.neuflow.org).

## this package

This package is a compiler toolkit for neuFlow. It is entirely written in
[Lua](http://www.lua.org/), and relies on
[Torch7](https://github.com/andresy/torch) to represent N-dimensional arrays
efficiently. It also interfaces Torch7's neural-network package natively.

## how to install

Torch7 must be install first, a task most easily accomplished using the single
line [install script](https://github.com/clementfarabet/torchinstall).

or alternatively to install Torch7 and the neuFlow package by hand, you will
need to install a few dependencies.

On Linux (Ubuntu):

``` sh
$ apt-get install gcc g++ git libreadline5-dev cmake wget
$ apt-get install libqt4-core libqt4-gui libqt4-dev
$ apt-get install ffmpeg gnuplot
```

On Mac OS X (> 10.5): get [Homebrew](http://mxcl.github.com/homebrew/)
and then:

``` sh
$ brew install git readline cmake wget
$ brew install qt
$ brew install ffmpeg gnuplot
```

You're ready to install Torch7 (www.torch.ch). The most up to date instructions
can be found at the [Torch7 github page](https://github.com/andresy/torch).

``` sh
$ git clone git://github.com/andresy/torch.git
$ cd torch
$ mkdir build
$ cd build

$ cmake ..
OR
$ cmake .. -DCMAKE_INSTALL_PREFIX=/my/install/path
```

Or if you already have a previous Torch7 installed:

``` sh
$ luarocks install torch WITH_LUA_JIT=1 # Torch7, an efficient numeric library for Lua
```

You will also need additional packages:

``` sh
$ luarocks install image        # an image library for Torch7
$ luarocks install nnx          # lots of extra neural-net modules
$ luarocks install camera       # a camera interface for Linux/MacOS
$ luarocks install ffmpeg       # a video decoder for most formats
$ luarocks install inline-c     # inline C capability
```

Now that Torch7 has been installed the neuflow package can be installed.
Installing the neuflow package requires you to download the source code
repository. It'll give you access to some demos, to get started:

``` sh
$ git clone https://github.com/clementfarabet/neuflow.git
$ cd neuflow
$ luarocks make
```

## how to run code on neuFlow

Demos are located in demos/. To get started, you'll need
a standard Xilinx dev board for the Virtex 6: [the ML605 Kit]
(http://www.xilinx.com/products/devkits/EK-V6-ML605-G.htm).
We provide an image of neuFlow that's pre synthesized/mapped/routed
for the Virtex6 VLX240T on this platform.

To run any of the demos, follow these instructions (tested on
Ubuntu 9.04, 10.04 and Mac OS X 10.5, 10.6 and 10.7).

``` sh
$ git clone https://github.com/clementfarabet/neuflow.git
$ cd neuflow

# make Xilinx tools available (it implies you have them
# installed somewhere...)
$ source $XILINX_INSTALL_PATH/settings**.sh

# turn on the ML605, plug the JTAG cable then load one of
# our pre-built bitfiles *:
$ cd scripts
$ ./get-latest-neuflow-image
$ ./load-bitfile neuFlow-ml605.bit

# at this points, you just have wait 2 seconds that the Ethernet
# LEDs are back on (out of reset)

# run the simplest demo, a loopback client, to verify your setup **:
$ cd ../demos
$ sudo torch loopback.lua # on Linux
or
$ ./loopback.lua # on OSX

# before loading a new demo, you have to reset neuFlow: for
# now it is done by pressing the SW10 button (cpu rst)

# then you can run a typical convnet-based program, a face detector:
$ sudo torch face-detector.lua # on Linux
or
$ ./face-detector.lua # on OSX
```

(*) the load-bitfile script assumes that you have properly installed Xilinx's
USB cable driver. On RedHat and derivatives it works out of the box when
installing Xilinx ISE, but on Ubuntu you'll have to follow these instructions:
http://rmdir.de/~michael/xilinx/.  This is not doable on Mac OS X
unfortunately. I usually flash the ML605 board using Ubuntu (even a virtual box
version works), and then run all the demos under Mac OS X.

(**) you need to have admin privileges on your machine (sudo) to be able to
interact with neuFlow, as we're using a custom low-level Ethernet framing
protocol.
