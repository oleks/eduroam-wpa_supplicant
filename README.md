# Sample wpa_supplicant configuration for eduroam

After some time wrestling with [eduroam](www.eduroam.org), I seem to have found
[this `wpa_supplicant` configuration](supplicant.conf) to be rather robust.

## Last confirmed to work at

* Den Sorte Diamant, Copenhagen, Denmark: September 2016.
* DIKU, Copenhagen, Denmark: September 2016.
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
