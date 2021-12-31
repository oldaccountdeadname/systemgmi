# systemgmi
low-configuration systemd monitoring over gemini.

## Overview
systemgmi is a tool for remote server monitoring written in C. You can think of
it as sort of like `systemctl status` served over Gemini. It latches on to the
configuration of systemd, and, as such, requires no configuration of its own
outside of a TLS certificate and a few environment variables.

## Usage
Enable/start the systemgmi service, giving it a TLS certificate in environment
variables (see `--help` for which env vars exactly). If you navigate to your IP,
you'll see systegmi's index. It contains the following:

+ the hostname
+ a little neofetch run (if neofetch is in PATH)
+ a link to the system journal (if permissions are present)
+ a list of units

If you follow the link for a unit, you'll see information roughly equivalent to
running `systemctl status <unit>`: is it running, how much RAM is it using, etc.
If systemgmi is running as a user with the proper permissions, you can also view
the journal for that unit.

## Goals & Scope
Systemgmi should slot into any server environment with very little effort (or,
ideally, no effort at all). If people are doing crazy things with this, it's
built incorrectly. Working around its environment, systemgmi should never crash
or error out at runtime (excluding pre-flight checks) and never exhibit
unexpected behavior. Some environment variables and `systemctl enable` is all
that should be required to have an instance of systemgmi in *any case*.

## Contributing
Do not be afraid to deform systemgmi beyond recognition! And, if you'd like,
definitely do not be afraid to contribute those changes (even highly opinionated
ones) upstream! If you have Nix installed already and want a quick development
environment, `nix develop` will place you in a shell with a completely working
build and test environment including libraries and down to a sacrificial TLS
certificate pointed to by the proper environment variables.

If you want a style guide, I roughly adhere to Suckless' style. The only part of
that which is actually important is that headers are not to be included by other
headers. Only individual compilation units may `#include` things. Other than
that, just make a good faith effort to keep the code you write homogeneous with
what's already there. I find this style keeps individual units nice and isolated
and encapsulated, which is particularly helpful when working with 'heavy'
bindings like that of dbus and OpenSSL.

If you want something to work on, you can `git grep` for `TODO`s in the code
base, or take a look at this list of things I haven't gotten to yet and/or fully
considered:

+ The error messages aren't currently that great. While *some* of the issues are
  just copy, the real issue is that the causes of errors are ignored, and we
  don't check things like `errno`.
+ Systemgmi can't be bound to listen on any IP other than 0.0.0.0. It would make
  sense to refactor this into an environment variable.
+ Systemgmi can't control processes. Adding some links to start, restart, stop,
  enable, and disable units could be a good idea. Maybe.
+ Logging has no unified approach. Systemgmi just sort of logs whatever it feels
  like without considering why. Someone help.
+ Systemgmi doesn't quite have a pedantic interpretation of the draft spec. This
  is fine in practice, but if any purists want to give the implementation a look
  and go over any rough spots, it would be greatly appreciated.
+ Systemgmi gives little insight into and control of itself. Maybe at some point
  an admin console providing access to things like cache management, etc, could
  be useful.

## Licensing
systemgmi is Free Software which you may distribute under the terms of the GNU
General Public License as published by the FSF and distributed in the LICENSE
file at the root of this source tree.

In addition, as a special exception, the copyright holders provide permission to
link the code residing in this file tree with the OpenSSL library. You must obey
the GNU General Public License in all respects for all code used other than
OpenSSL. If you modify file(s) with this exception, may extend this exception to
your file(s), but you are not obligated to do so. If you do not wish to do so,
remove these paragraphs declaring the exception from your version.
