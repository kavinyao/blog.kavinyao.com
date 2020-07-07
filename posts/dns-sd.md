title: Discover Local Chromecast Devices Using DNS-SD
date: 2020-07-04

---

I always wondered how Chromecast devices are magically detected by different apps. And Spotify allows me to play music from my Sony SRS-X77 speaker. How does Spotify even know that speaker exists?

Well, it turns out this is powered by [DNS Service Discovery](https://en.wikipedia.org/wiki/Zero-configuration_networking#DNS-SD). In fact you can use command line tools to [emulate the discovery process](https://apple.stackexchange.com/a/239039/91868). macOS has `dns-sd` tool builtin (for other OS you can use [Avahi](https://www.avahi.org/)).

You start the service discovery process with this command:

    % dns-sd  -B  _services._dns-sd._udp  local.
    Browsing for _services._dns-sd._udp.local.
    DATE: ---Sat 04 Jul 2020---
    22:39:00.155  ...STARTING...
    Timestamp     A/R    Flags  if Domain               Service Type         Instance Name
    22:39:00.156  Add        3   4 .                    _tcp.local.          _http
    22:39:00.156  Add        3   4 .                    _tcp.local.          _spotify-connect
    22:39:00.156  Add        2   4 .                    _tcp.local.          _companion-link
    22:39:00.397  Add        3   4 .                    _tcp.local.          _googlezone
    22:39:00.397  Add        2   4 .                    _tcp.local.          _googlecast
    22:39:00.397  Add        2   1 .                    _tcp.local.          _companion-link
    22:39:00.499  Add        3   4 .                    _tcp.local.          _nvstream_dbd
    22:39:00.802  Add        3   4 .                    _tcp.local.          _printer
    22:39:00.802  Add        2   4 .                    _tcp.local.          _ipp
    22:39:00.932  Add        3   4 .                    _tcp.local.          _smb
    22:39:00.932  Add        3   4 .                    _tcp.local.          _device-info
    22:39:00.932  Add        2   4 .                    _tcp.local.          _afpovertcp
    22:39:01.238  Add        2   4 .                    _tcp.local.          _alexa

You can see some familiar names in it, e.g. Chromecast (googlecast), Spotify and Alexa.

Now let's see which devices are running Chromecast:

    % dns-sd -B _googlecast._tcp local.
    Browsing for _googlecast._tcp.local.
    DATE: ---Sat 04 Jul 2020---
    22:42:52.056  ...STARTING...
    Timestamp     A/R    Flags  if Domain               Service Type         Instance Name
    22:42:52.057  Add        3   4 local.               _googlecast._tcp.    Chromecast-b838<...>
    22:42:52.057  Add        3   4 local.               _googlecast._tcp.    Chromecast-df0d<...>
    22:42:52.057  Add        2   4 local.               _googlecast._tcp.    Chromecast-Ultra-1234<...>

Looks like on my local network there are 3 Chromecast devices. Let's get more information of the Chromecast Ultra:

    % dns-sd -L Chromecast-Ultra-1234<...> _googlecast._tcp local.
    Lookup Chromecast-Ultra-1234<...>._googlecast._tcp.local.
    DATE: ---Sat 04 Jul 2020---
    22:44:11.610  ...STARTING...
    22:44:11.611  Chromecast-Ultra-1234<...>._googlecast._tcp.local. can be reached at 1234d567-89ab-cdef-1234-56789abcdef0.local.:8009 (interface 4)
     id=1234<...> cd=43DD<...> rm= ve=05 md=Chromecast\ Ultra ic=/setup/icon.png fn=Dining\ Table ca=200709 st=0 bs=FA8FCA5D4DCF nf=1 rs=

You can see a bunch of metadata, including the user assigned device name (Dining Table). Also notice the address in the "can the reached at" section. Use it we can get the IP of the device:

    % dns-sd -G v4v6 1234d567-89ab-cdef-1234-56789abcdef0.local.
    DATE: ---Sat 04 Jul 2020---
    22:48:15.421  ...STARTING...
    Timestamp     A/R    Flags if Hostname                                    Address                                      TTL
    22:48:15.421  Add 40000002  4 1234d567-89ab-cdef-1234-56789abcdef0.local. 192.168.1.71                                 120

Bingo. Its IP is `192.168.1.71`. And now you know how the googlecast library locates the Chromecast devices.
