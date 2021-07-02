title: How to Fix UniFi AP Unable to Add Issue
date: 2021-07-02
type: secondary

---

I bought 2 UniFi FlexHD APs a year ago and so far they have been working really well. That said, it's unrealistic to expect prosumer electronic to have infinite uptime. For these APs, they can get into a limbo state: clients are dropped and in controller they are stuck in "Unable to Add" state. This issue only has happened once for each AP but it does seem to reproduce consistently given enough uptime.

When an AP gets into this state, you'll have no luck recovering them from the controller. The only viable fix I found is to reset the AP and adopt it again as if they are new. There are 2 ways to reset: via the hardware reset button or via SSH. Here I have to complain about the physical reset button on FlexHD AP. The hole is simply too deep and I cannot find a common object that's thin and long enough to reach the button. As a result I had to resort to SSH.

To get into the UniFi OS console via SSH, try the default username/password combination (`ubnt/ubnt`) first. If it doesn't work, it means the controller setting is still persisted in the AP, even after it's forgotten by the controller. You can find the controller issued SSH credential from the "Device SSH Authentication" settings. Once you get into the console, run this command `sudo syswrapper.sh restore-default` and the AP will be reset. After a minute or so the AP will appear in the controller, ready for adoption again.
