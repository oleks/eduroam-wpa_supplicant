# Sample wpa_supplicant configuration for eduroam

After some time wrestling with [eduroam](www.eduroam.org), I seem to have found
this `wpa_supplicant` configuration to be rather robust.

## Last confirmed to work at

* DIKU, Copenhagen, Denmark: August 2016.
* DTU, Lyngby, Denmark: August 2016.
* Den Sorte Diamant, Copenhagen, Denmark: August 2016.
* Stanford University, Palo Alto, USA: June 2016.
* University of Oregon, Eugene, USA: June 2016.
* Oxford, United Kingdom: July 2015.
* ITU, Copenhagen, Denmark: May 2015.

## Usage

See [supplicant.conf](supplicant.conf).

Change identity and password to your eduroam account.

## Quick and Dirty start-Up

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

Optionally, use the `-B` option to move the `wpa_supplicant` process to background.

Additionally, start up `dhcpcd` if it doesn't start automatically.
