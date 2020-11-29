title: Temperature in Ubuntu MOTD
date: 2020-11-28
type: secondary

---

I recently built a home server with spare parts and installed Ubuntu server on it. I want to make sure the temps are good since the server is going to run 24/7. Besides the `sensors` command, I noticed that the [MOTD](https://en.wikipedia.org/wiki/Motd_(Unix)) also reports a temperature. Like this:

    System information as of Fri 27 Nov 2020 10:56:18 AM UTC
    
    System load:  0.0                Temperature:     36.0 C
    Usage of /:   1.4% of 456.96GB   Processes:       168
    Memory usage: 2%                 Users logged in: 0
    Swap usage:   0%                 IPv4 address:    192.168.1.111

This is pretty neat but what temperature is it? And what API is used to get it?

A quick Google search led us to [how MOTD is generated](https://askubuntu.com/questions/319528/how-to-see-the-details-which-ubuntu-shows-at-the-time-of-login-anytime#comment1635612_939579). The script that generates the system information part is `/etc/update-motd.d/50-landscape-sysinfo`, which calls `/usr/bin/landscape-sysinfo`.

It turns out this is a Python script:

    $ file /usr/bin/landscape-sysinfo
    /usr/bin/landscape-sysinfo: Python script, ASCII text executable

What it does is basically calling `landscape.sysinfo.deployment.run`. We can find the file from Python repl:

    >>> from landscape.sysinfo import deployment
    >>> deployment.__file__
    '/usr/lib/python3/dist-packages/landscape/sysinfo/deployment.py'

In `deployment.py` we can find all the plugins and the temperature plugin is from `temperature.py`, which calls `landscape.lib.sysstats.get_thermal_zones` and reports the highest temperature. If we open `sysstats.py` we can find the definition of this function:

    def get_thermal_zones(thermal_zone_path=None):
        if thermal_zone_path is None:
            if os.path.isdir("/sys/class/thermal"):
                thermal_zone_path = "/sys/class/thermal"
            else:
                thermal_zone_path = "/proc/acpi/thermal_zone"
        if os.path.isdir(thermal_zone_path):
            for zone_name in sorted(os.listdir(thermal_zone_path)):
                yield ThermalZone(thermal_zone_path, zone_name)

We can use it in action:

    >>> from landscape.lib.sysstats import get_thermal_zones
    >>> tzs = get_thermal_zones()
    >>> for tz in tzs:
    ...   print(tz.path, tz.temperature)
    ...
    /sys/class/thermal/cooling_device0 None
    /sys/class/thermal/cooling_device1 None
    /sys/class/thermal/cooling_device2 None
    /sys/class/thermal/cooling_device3 None
    /sys/class/thermal/cooling_device4 None
    ...
    /sys/class/thermal/thermal_zone0 27.8 C
    /sys/class/thermal/thermal_zone1 29.8 C
    /sys/class/thermal/thermal_zone2 21.0 C

But what is a thermal zone? It turns out this is all from the [Sysfs API](https://www.kernel.org/doc/Documentation/thermal/sysfs-api.txt) . Per the documentation, a thermal zone is basically a sensor. We can actually look at the content in `thermal_zone_path` (`/sys/class/thermal` in my case) to see what sensor it is. Take `thermal_zone2` as example:

    $ cat /sys/class/thermal/thermal_zone2/type
    x86_pkg_temp

This one is the CPU package temperature.

In short, the temperature reported in MOTD is the highest temperature of all thermal zones reported by Sysfs API, *at login*. For more comprehensive temperature reporting you still want to use `sensors`:

    $ sensors
    acpitz-acpi-0
    Adapter: ACPI interface
    temp1:        +27.8°C  (crit = +119.0°C)
    temp2:        +29.8°C  (crit = +119.0°C)
    
    coretemp-isa-0000
    Adapter: ISA adapter
    Package id 0:  +25.0°C  (high = +80.0°C, crit = +100.0°C)
    Core 0:        +19.0°C  (high = +80.0°C, crit = +100.0°C)
    Core 1:        +21.0°C  (high = +80.0°C, crit = +100.0°C)
    Core 2:        +20.0°C  (high = +80.0°C, crit = +100.0°C)
    Core 3:        +20.0°C  (high = +80.0°C, crit = +100.0°C)
