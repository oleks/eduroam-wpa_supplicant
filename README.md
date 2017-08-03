# Sample wpa_supplicant configuration for eduroam

TLDR; After some time wrestling with [eduroam](www.eduroam.org), I seem to have
found [this `wpa_supplicant` configuration](supplicant.conf) to be rather
robust.

[Eduroam](https://www.eduroam.org) is a secure, wireless access service made
available to the education and research community by many educational
institutions around the world. It was designed so that you as a student or
researcher have to exert minimal effort to connect to a secure wireless
network, no matter which educational institution you happen to be at that day.
This encourages educational exchange, and scientific collaboration around the
world. ([This video](https://www.youtube.com/watch?v=TVCmcMZS3uA) explains
eduroam using cartoons!)

[`wpa_supplicant`](https://linux.die.net/man/8/wpa_supplicant) is a generic
["IEEE 802.1X supplicant"](https://en.wikipedia.org/wiki/Supplicant_(computer))
(i.e., the tool that can make sure your wireless connection is secure). Most
Linux-based networking managers use `wpa_supplicant` behind the scenes. Of
course, `wpa_supplicant` has a command-line interface, and it is fairly
straight-forward to exert grand control over your configuration. (There are no
cartoons about `wpa_supplicant` ☹.)

To this end, it is a shame that [the generic eduroam web-site](www.eduroam.org)
seemingly (i.e., correct me if I'm wrong) offers no documentation on how to set
up your `wpa_supplicant`. Instead, they offer installers to end-users,
including a shell-script for Linux users (which could be regarded as primitive,
but honest documentation). Some institutions do offer raw `wpa_supplicant`
documentation, but do so in an ad-hoc fashion (i.e., without any guarantee that
the configuration will work at another institution).

This is an attempt to establish a unified `wpa_supplicant` configuration, that
works across the board. For now however, this is just an undocumented
[`wpa_supplicant` configuration](supplicant.conf) that seems to work rather
well across a number of institutions. Lend a hand, and document it, or just let
me know if this configuration also works for you.

## Last confirmed to work at

* University of California, Berkeley, USA: August 2017
* Malmö Airport, Sweden: July 2017.
* DIKU, Copenhagen, Denmark: July 2017.
* University of Budapest, Hungary: May 2017.
* Oslo Airport, Norway: April 2017.
* Sapienza, University of Rome, Italy: November 2016.
* RISC Institute, Pond Building, Hagenberg, Austria: September 2016.
* Den Sorte Diamant, Copenhagen, Denmark: September 2016.
* DTU, Lyngby, Denmark: August 2016.
* Stanford University, Palo Alto, USA: June 2016.
* University of Oregon, Eugene, USA: June 2016.
* Oxford, United Kingdom: July 2015.
* ITU, Copenhagen, Denmark: May 2015.

## Usage

1. See [supplicant.conf](supplicant.conf).
2. Change identity and password hash to match your eduroam account.

The password hash needs to be an md4 hash of the little-endian UTF16 encoding
of your password. For instance, if your password is `hamster`, you can hash it
as follows:

~~~
$  echo -n "hamster" | iconv -t utf16le | openssl md4
~~~

(See also [the `HISTCONTROL` bash
variable](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-HISTCONTROL)
for keeping commands out of your `~/.bash_history`.)

## Quick and dirty start-up

If you prefer to roll without a network manager, here is the quick and dirty
way to run `wpa_supplicant` with this config:

~~~
$ sudo wpa_supplicant -Diwlwifi -iwlp3s0 -c supplicant.conf -B
~~~

Where `iwlwifi` is the kernel driver stated for your wireless card. (`wext`
is a deprecated driver that often works as well.) You can find your standard
driver using `lspci`:

~~~
$ lspci -k
~~~

`wlp3s0` is the network interface name for your wireless card. You can find
this using `ip link`:

~~~
$ ip link
~~~

Optionally, use the `-B` option to move the `wpa_supplicant` process to
background. Leaving it out, however, provides you with useful insights if you
otherwise cannot connect.

Additionally, start up `dhcpcd` if it doesn't start automatically.
