---
layout: post
title: "Using a Segger JLink with OS X"
date: 2012-03-29 10:28
comments: true
categories: 
---
I wanted to work on some ARM Cortex M3 programming on my Mac...

In order to program my dev boards, I use a <a href="http://www.segger.com/j-link-edu.html">Segger J-Flash</a>.  At first glance, they don't appear to support the hardware on OS X.  However, after some digging I found that they offer a beta that includes OS X support <a href="http://www.segger.com/beta-software-version.html">here</a>

Once I got that installed, I received the following error:

```
dyld: Library not loaded: /usr/local/lib/libusb-1.0.0.dylib
  Referenced from: /Users/draphael/JLink/./JLinkGDBServer
  Reason: image not found
Trace/BPT trap: 5
```

Well, of course that just means I need libusb installed.  I use homebrew for my ports of linux to mac.  

```
brew install libusb
```

This worked like a champ...except...now I get this error:

```
dyld: Library not loaded: /usr/local/lib/libusb-1.0.0.dylib
  Referenced from: /Users/draphael/JLink/./JLinkGDBServer
  Reason: no suitable image found.  Did find:
	/usr/local/lib/libusb-1.0.0.dylib: mach-o, but wrong architecture
	/usr/local/lib/libusb-1.0.0.dylib: mach-o, but wrong architecture
Trace/BPT trap: 5
```

D'OH!  I've seen this before...time to edit the formula

```
brew edit libusb
```

I needed to add the i386 arch flag as shown in this code:

``` ruby
require 'formula'

class Libusb < Formula
  url 'http://downloads.sourceforge.net/project/libusb/libusb-1.0/libusb-1.0.8/libusb-1.0.8.tar.bz2'
  homepage 'http://www.libusb.org/'
  md5 '37d34e6eaa69a4b645a19ff4ca63ceef'
  head 'git://git.libusb.org/libusb.git'

  def options
    [["--universal", "Build a universal binary."]]
  end

  if ARGV.build_head? and MacOS.xcode_version >= "4.3"
    depends_on "automake" => :build
    depends_on "libtool" => :build
  end

  def install
    ENV.universal_binary if ARGV.build_universal?
    # Added flag to compile libusb for i386
    ENV["CFLAGS"] += " -arch i386"
    system "./autogen.sh" if ARGV.build_head?
    system "./configure", "--prefix=#{prefix}", "--disable-dependency-tracking"
    system "make install"
  end
end
```

Once that was complete...

``` bash
~/JLink $ ./JLinkGDBServer 
SEGGER J-Link GDB Server V4.43c (beta)

JLinkARM.dll V4.43c (DLL compiled Feb 22 2012 20:13:03)

The server has been started with the following settings:
---Server related settings---
GDBInit file:              none
Listening port:            2331
SWO thread listening port: 2332
Accept remote connection:  yes
Logfile:                   off
Verify download:           off
Init regs on start:        on
Silent mode:               off
Single run mode:           off
---J-Link related settings---
J-Link script:             none
Target interface:          JTAG
Host interface:            USB
Target endian:             big
Target interface speed:    5kHz

Waiting for J-Link connection...
J-Link is connected.
```

Success!!!
