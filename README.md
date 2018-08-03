# Sample wpa_supplicant configuration for eduroam

TLDR; [This `wpa_supplicant` configuration](supplicant.conf) for
[eduroam](https://www.eduroam.org) [seems to be rather
robust](#last-confirmed-to-work-at).

[Eduroam](https://www.eduroam.org) is a secure, wireless access
service made available to the education and research community by many
educational institutions around the world. It was designed so that you
as a student or researcher have to exert minimal effort to connect to
a secure wireless network, no matter which educational institution you
happen to be at today.  This encourages educational exchange and
scientific collaboration around the world. ([This
video](https://www.youtube.com/watch?v=TVCmcMZS3uA) explains eduroam
using cartoons!)

[`wpa_supplicant`](https://linux.die.net/man/8/wpa_supplicant) is a generic
["IEEE 802.1X supplicant"](https://en.wikipedia.org/wiki/Supplicant_(computer))
(i.e., the tool that can make sure your wireless connection is secure). Most
Linux-based networking managers use `wpa_supplicant` behind the scenes. Of
course, `wpa_supplicant` has a command-line interface, and it is fairly
straight-forward to exert grand control over your configuration. (There are no
cartoons about `wpa_supplicant` ☹.)

To this end, it is a shame that [the generic eduroam
web-site](https://www.eduroam.org) seemingly (i.e., correct me if I'm wrong)
offers no documentation on how to set up your `wpa_supplicant`. Instead, they
offer installers to end-users, including a shell-script for Linux users (which
could be regarded as primitive, but honest documentation). Some institutions do
offer raw `wpa_supplicant` documentation, but do so in an ad-hoc fashion —
**without any guarantee that the configuration will work at any other
institution, defeating the purpose of Eduroam**

This is an attempt to establish a unified `wpa_supplicant` configuration, that
works across the board. For now however, this is just an undocumented
[`wpa_supplicant` configuration](supplicant.conf) that seems to work rather
well across a number of institutions. Lend a hand, and document it, or just let
me know if this configuration also works for you.

## Last confirmed to work at

* Federal University of Santa Catarina, Florianópolis, Brazil: August 2018
  ([@gus9182](https://github.com/gus9182))
* University of Canterbury, Christchurch, New Zealand: July 2018
  ([@huba](https://github.com/huba))
* University of Waikato, Hamilton, New Zealand: July 2018
  ([@huba](https://github.com/huba))
* Amsterdam University Library, Netherlands: July 2018
  ([@oleks](https://github.com/oleks))
* The Hong Kong Polytechnic University, Hong Kong: June 2018
  ([@tobychui](https://github.com/tobychui))
* TU Wien, Vienna, Austria: June 2018
  ([@thrau](https://github.com/thrau))
* University of York, United Kingdom: May 2018
  ([@bendudson](https://github.com/bendudson))
* Grenoble INP, France: May 2018
  ([Frédéric Pétrot](http://tima.imag.fr/sls/people/petrot/))
* University of Glasgow, Scotland: April 2018
  ([@manaratz](https://github.com/manaratz))
* ESEO Angers, France: March 2018
  ([@gondyb](https://github.com/gondyb))
* Kaunas Technology Universty (KTU), Lithuania: March 2018
  ([@Mark-Weston](https://github.com/Mark-Weston))
* University of Brighton, United Kingdom: March 2018
  ([@DavidAveryUoB](https://github.com/DavidAveryUoB))
* Aalto University, Finland: February 2018
  ([@Niketin](https://github.com/Niketin))
* University of Cape Town, South Africa: February 2018
  ([@riazarbi](https://github.com/riazarbi))
* University of Cambridge, United Kingdom: February 2018
  ([@rspencer01](https://github.com/rspencer01))
* University of Sheffield, United Kingdom: January 2018
  ([@ewnh](https://github.com/ewnh))
* INSA Lyon, France: January 2018
  ([@sfrenot](https://github.com/sfrenot))
* Univeristy of Oslo, Norway: January 2018
  ([@oleks](https://github.com/oleks))
* University of Copenhagen, Denmark: January 2018
  ([@oleks](https://github.com/oleks))
* California State University, Sacramento, USA: December 2017
  ([@leaptthroughtime](https://github.com/leaptthroughtime))
* University of California, Berkeley, USA: August 2017
  ([@wizh](https://github.com/wizh))
* Malmö Airport, Sweden: July 2017
  ([@oleks](https://github.com/oleks))
* University of Budapest, Hungary: May 2017
  ([@oleks](https://github.com/oleks))
* Oslo Airport, Norway: April 2017
  ([@oleks](https://github.com/oleks))
* Sapienza, University of Rome, Italy: November 2016
  ([@Enrico204](https://github.com/Enrico204))
* RISC Institute, Pond Building, Hagenberg, Austria: September 2016
  ([@oleks](https://github.com/oleks))
* Den Sorte Diamant, Copenhagen, Denmark: September 2016
  ([@oleks](https://github.com/oleks))
* DTU, Lyngby, Denmark: August 2016
  ([@oleks](https://github.com/oleks))
* Stanford University, Palo Alto, USA: June 2016
  ([@oleks](https://github.com/oleks))
* University of Oregon, Eugene, USA: June 2016
  ([@oleks](https://github.com/oleks))
* Oxford, United Kingdom: July 2015
  ([@oleks](https://github.com/oleks))
* ITU, Copenhagen, Denmark: May 2015
  ([@oleks](https://github.com/oleks))

## Usage

1. See [supplicant.conf](supplicant.conf).
2. Set `identity` to `abc123@ku.dk`, if your username is
   `abc123`, and your home university domain is `ku.dk`.
3. Similarly, set the `anonymous_identity` to either `anonymous@ku.dk`
   or simply `@ku.dk`. Using an anonymous identity does not reveal your
   identity to anyone but the home university — eduroam calls home to
   verify your identity and password every time you login from another
   location.
4. Set the password hash to match your university password (see below).

The password hash needs to be an MD4 hash of the little-endian UTF16 encoding
of your password. For instance, if your password is `hamster`, you can hash it
as follows:

~~~
$  echo -n 'hamster' | iconv -t utf16le | openssl md4
~~~

(Note the use of single-quotes to avoid escaping in the shell.)

(See also [the `HISTCONTROL` bash
variable](https://www.gnu.org/software/bash/manual/html_node/Bash-Variables.html#index-HISTCONTROL)
for keeping commands out of your `~/.bash_history`.)

If you are using [`pass`](https://www.passwordstore.org/), or another
password-manager with a command-line interface, you might consider
a pipeline like this instead:

~~~
$  pass eduroam | tr -d '\n' | iconv -t utf16le | openssl md4
~~~

Once you have the MD4 hash, write it into your configuration as
follows:

~~~
  password=hash:2fd23a...456cef
~~~

**NB!** MD4 is [an _obsolete_ hashing
algorithm](https://tools.ietf.org/html/rfc6150) and should not be
considered secure.

## Quick and dirty start-up

If you prefer to roll without a network manager, here is the quick and dirty
way to run `wpa_supplicant` with this config:

~~~
$ sudo wpa_supplicant -Dnl80211 -iwlp3s0 -c supplicant.conf -B
~~~

Where `nl80211` is the kernel driver to use.
[`nl80211`](https://wireless.wiki.kernel.org/en/users/drivers/nl80211)
is the new default 802.11 netlink interface, intended to replace the
older `wext` (Wireless-Extensions). If you do not have `nl80211` lying
around, you may try `wext`, but `wext` can fail with the error
`ioctl[SIOCGIWSCAN]: Argument list too long` in the face of too many
access points. If you have an Intel card, another alternative is
[`iwlwifi`](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi).

One way to find the driver you need is using `lspci`:

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

## Platform-specific configurations

### Raspbian Stretch

On [Raspbian
Stretch](https://www.raspberrypi.org/blog/raspbian-stretch/) you would
also have to add the following lines (courtesy of
[@patrick-nits](https://github.com/patrick-nits)):

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=root
country=<2-letter country code>
```

  * `ctrl_interface` is needed because Raspbian Stretch uses
    `wpa_cli` by default. `ctrl_interface` is needed whenever you
    use `wpa_cli`.
  * `country` is needed ["for regulatory purposes"](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md).
    In particular, this alters the frequency bands that `wpa_supplicant`
    will use. The country code must be an [ISO 3166-1 Alpha-2 Code](https://en.wikipedia.org/wiki/ISO_3166-1).
