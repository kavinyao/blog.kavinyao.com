title: The NUC Experience
date: 2020-01-29

---

One year after my [failed Surface Go as living room PC experiment](/posts/surface-go), I'm still living in the pain of having to run down stairs to cast a tab to my living room TV. I seem to have run out of options since so far I have crossed out iPad (Chrome iOS app doesn't support tab cast), Windows tablet (not powerful enough), cheap Windows laptop (bulky for living room use, short battery life) and ITX PC (bulky, overkill even with lowest spec, potential thermal issue).

The other day I suddenly realized that an Intel NUC could be a perfect solution. For those who don't know what a NUC is, NUC stands for Next Unit of Computing and is basically an ultra small-form-factor (SFF) PC. Normally when people talk about SFF PC they typically refer to the ITX form factor, which is 170mm×170mm. The NUC form factor is even smaller, at 100mm×100mm. A NUC uses low power-consuming mobile CPUs which are very easy to cool without sacrificing any necessary feature. Take [NUC8i3BEK](https://www.intel.com/content/www/us/en/products/boards-kits/nuc/kits/nuc8i3bek.html) as an example, it has 3 USB ports, 1 Thunderbolt 3 port (supports DP 1.2), 1 HDMI 2.0 port, 1Gb LAN, 802.11ac WiFi, Bluetooth 5.0, 1 headphone jack, dual microphones and even a microSD card reader. Internally it supports SODIMM DDR4 (dual channel, up to 32GB 2400MHz), NVMe M.2 SSD and SATA. That's basically everything you can have with any latest PC. The only thing it lacks is a discrete graphics card, which you can make up with an external GPU using Thunderbolt 3 if you really need.

After some research I decided the aforementioned NUC8i3BEK kit should be enough for my use case. It's priced at $309 and features a mobile 8th-gen i3-8109U CPU, which has 2 cores and 4 threads @ 3.0GHz (can turbo boost to 3.6GHz). There are newer or more powerful NUCs available but since my use case is so simple there's no need to waste money. NUC8i3BEK is a base kit so I need to add my own RAM and storage. After pairing it with 16GB DDR4 2400 MHz (8GB×2 kit in order to enable dual channel) and a 250GB NVMe SSD, the total price is around $430. This is $40 more expensive than the base Surface Go model but its performance is monstrous compared to that. To put things into comparison:

<table>
  <tbody>
    <tr>
      <td colspan="1" rowspan="1">
      </td>
      <td colspan="1" rowspan="1">
        Surface Go Base Model
      </td>
      <td colspan="1" rowspan="1">
        My NUC
      </td>
      <td>
        Relative Performance
      </td>
    </tr>
    <tr>
      <td colspan="1" rowspan="1">
        CPU
      </td>
      <td colspan="1" rowspan="1">
        Intel Pentium Gold Processor 4415Y
      </td>
      <td colspan="1" rowspan="1">
        Intel Core i3-8109U
      </td>
      <td colspan="1" rowspan="1">
        <a href="https://cpu.userbenchmark.com/Compare/Intel-Pentium-Gold-4415Y-vs-Intel-Core-i3-8109U/m549016vsm609620">2.4X faster</a>
      </td>
    </tr>
    <tr>
      <td colspan="1" rowspan="1">
        GPU
      </td>
      <td colspan="1" rowspan="1">
        Intel HD Graphics 615
      </td>
      <td colspan="1" rowspan="1">
        Intel Iris Plus Graphics 655
      </td>
      <td colspan="1" rowspan="1">
        <a href="https://gpu.userbenchmark.com/Compare/Intel--Iris-Plus-650-Mobile-Kaby-Lake-vs-Intel-HD-615-Mobile-Kaby-Lake/m367939vsm193629">2.4X faster</a>
      </td>
    </tr>
    <tr>
      <td colspan="1" rowspan="1">
        RAM
      </td>
      <td colspan="1" rowspan="1">
        4GB
      </td>
      <td colspan="1" rowspan="1">
        16GB (dual channel)
      </td>
      <td colspan="1" rowspan="1">
        4X bigger
      </td>
    </tr>
    <tr>
      <td colspan="1" rowspan="1">
        SSD
      </td>
      <td colspan="1" rowspan="1">
        64GB
      </td>
      <td colspan="1" rowspan="1">
        250GB
      </td>
      <td colspan="1" rowspan="1">
        4X bigger
      </td>
    </tr>
  </tbody>
</table>

Of course Surface Go has other nice features like touch screen but those are not relevant for my use case. Plus, since I can directly output video to my TV I don't need to bother with Chromecast, which works great but is nowhere as good as native video. I also got a Logitech K600 wireless keyboard with touch pad so I can do everything from my sofa.

I had a small hiccup during the setup. I initially bought Corsair 2400MHz SODIMM 8GB×2 kit but it turned out it's incompatible with this NUC and therefore the NUC couldn't boot. Googling reveals that people reported the same problem on [Reddit](https://www.reddit.com/r/intelnuc/comments/b7018m/warning_about_corsair_ram/). Ouch! I should've done some more research beforehand. As suggested by the Reddit post I switched to another kit from Crucial and the system boots without a problem.

So does everything work out? Yes, it does. To give some numbers,

* Single 1080P60 stream playback: CPU usage 10%, CPU temp 40°C, GPU usage 18%
* Four 720P60 streams playback: CPU usage 40%, CPU temp 50°C, GPU usage 50%

I was initially worried about thermals since I put the NUC in a closed cabinet but the thermal numbers are absolutely fine, thanks to the low TDP.

It's been a week with my NUC as living room PC and I am 100% satisfied.
