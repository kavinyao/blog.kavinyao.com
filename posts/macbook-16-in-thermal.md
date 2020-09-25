title: How to Tame Your 16" MacBook Pro
subtitle: and How to Thunderbolt
date: 2020-09-23

---

## Go Small and Go Big Again

I have been using a 13" MacBook Pro for more than 3 years. I upgraded from a 15" MacBook Pro for the reduced weight because jumping between meetings carrying the 15" wasn't fun. Since I mostly do the development on a remote server, the only real thing I do with the MacBook is web browsing using Chrome.

I was running a 15" portable monitor as the primary display when COVID WFH started. Realizing the situation was getting worse and WFH wasn't going to end anytime soon, I switched to a more serious setup with Caldigit TS3+ Thunderbolt 3 dock and LG UltraFine 22" 4K monitor.

It might or might not be a coincidence (which I'll revisit later in this article) but anyway my MacBook started to struggle even just doing moderate web browsing. I was actually waiting for the rumored 14" model but despite the amount of rumors there was no sign it would actually be released soon. Plus Apple announced 13" MacBook Pro refresh in May, which sealed the deal. Since my suspicion of the degraded performance is hardware (the 13" rocks a dual-core i7 processor without a discrete graphics card) and now I don't need to move between meetings (thanks Conoravirus?), I went with the all new 16" model with i7 processor (8-core 9980HK, big upgrade from 7567U) and a discrete GPU (AMD Radeon Pro 5500M to be accurate) this time. On paper this MacBook should have absolutely zero problem handling web browsing. But as it turns out, there's a catch.

## The Thermal Nightmare

**At the very first second I started using this MacBook, one problem was obvious: it's loud.** To be fair, for thin laptops like MacBooks, high RPM fans are a must have in order to keep CPU and GPU temperatures in check under load. However, when the system is idle the fan noise should be inaudible as it should be spinning at low RPM. I kept the exact same setup but switched the laptop. Clearly the new laptop is to blame. Since I have super low tolerance toward noise, I gotta find a solution.

There's also the possibility that I got a defective product but the amount of discussions on [discussions.apple.com](http://discussions.apple.com), Reddit and StackOverflow suggest otherwise. And the thermal problem is not specific to the 16" model: people reported similar fan noise problems on previous generations of 15" models too. As always there are lots of information you can find online but very few of them are actually useful. Suggested solutions (that seems legit) include resetting SMC, using the right thunderbolt ports, use integrated graphics (iGPU) instead of discrete graphics (dGPU) when possible. However none of them worked for me.

I was able to get a few things are confirmed though:

- When no external display is connected, you can choose between integrated graphics (iGPU) and discrete graphics (dGPU) to drive the internal display.
- When any external display is connected, dGPU will drive the external display and there's no way to turn this behavior off.
- There are apps that allows you to monitor temperatures of various components and set custom fan curves.

### Tuning the Fan Curve

Given my experience with PCs, the custom fan curve route seems promising since the default fan curve might just be unnecessarily too aggressive. So I downloaded [TG Pro](https://www.tunabellysoftware.com/tgpro/) which allows me to monitor temps and set custom fan speeds. AFAICT it only set fan speed steps instead of linear curves but it should suffice for my purpose. What I observed with TG Pro is that my CPU and GPU are running relatively hot at around 60°C at light system load. The temps are not great but should be far from thermal throttling (the T<sub>junction</sub> for the CPU, for example, is 100°C) and I would be happy to trade slightly higher idle temp for significantly lower fan noise. So I started with 20% fan duty if CPU temp is under 70°C. Boom! The MacBook became silent instantly. To quote my own twitter:

> My fan curve:
>
> - CPU < 70°C, 20% cycle
> - CPU > 70°C, 40% cycle
> - CPU > 80°C, 50% cycle
> - CPU > 90°C, 100% cycle
> - GPU > 80%, 50% cycle
>
> Result when watching video in Chrome, CPU usage@15-20%:
>
> - Before: CPU@65°C, fan@5000RPM, 47.5dBA
> - After: CPU@70°C, fan@3000RPM, 40.0dBA
>
> Day and night!

However, my happiness only lasted hours after I discovered that the MacBook started thermal throttling at idle load. This doesn't make sense because CPU temp looks totally fine. Obviously the custom fan speed setting is the culprit: once I change back to system fan speeds the thermal throttling stopped. Maybe my fan speed setting is too weak? A good spot I found is 25% always, 30% if CPU temp is above 65°C, 45% if above 70°C, etc. **This works most of the time. I still get occasional thermal throttling but overall the experience is much better than the default.**

### It's the Discrete GPU

I ran this setup for about a month, during which TG Pro even launched a new feature that auto switches to system fan speeds when thermal throttling is detected. It's not perfect but it does let me to do my job. But one day, I needed to do a presentation to my team. In order to present the slides I shared my screen. Everything was good until it was like half an hour into the presentation my MacBook started to thermal throttle again. Normally this is not a problem as system fan speeds will kick in and fix the problem in a few minutes. But this time I was live presenting to lots of people. The fan noise was so loud that it likely got picked up my the mic. Needless to say I was pretty embarrassed. I will probably do a few more presentations in the seemingly never-ending WFH era so I entered round 2 of fixing the thermals and fan noise.

The silver lining in this incident is that I got more signals to the thermal problem. So far I have been focusing on the CPU. Since I'm not doing any GPU intensive tasks like gaming or rendering, traditional wisdom says CPU should be the primary source of heat. *But what if it's not the case here? Thermal throttling didn't happen for weeks but as soon as I started sharing my screen, it was back.* To confirm the problem, I created a one-man video conference meeting, started screen sharing again and did some random things like moving cursors around, switching windows to emulate a presentation. And after a few minutes, CPU started to throttle – **I was able to reproduce the issue!** Remember the system behavior around external monitors? If I disconnect the external monitor and do the experiment again, there is NO thermal throttle. Similarly, if the lid is closed (so only external monitor is used), CPU and GPU temperatures are instantly 10-15°C lower. A quick Google search reveals [similar reports by other users](https://apple.stackexchange.com/q/342127/91868). At this point, it seems the dGPU is the problem. Or more precisely, **putting two power hungry, heat-generating components in a compact chassis without a competent cooling solution is the problem**.

Now that the problem is identified we can propose a few fixes. Since we definitely need to use the CPU, the trick is to reduce GPU load. And since dGPU is always used to drive external displays, the solution is simple: either not use external monitor or use external monitor only. This should work for most people but unfortunately not for me. I have been using dual monitor setup for years so switching to a single monitor setup will kill my productivity.

Fortunately, there is an alternative solution, albeit an expensive one.

## Enter eGPU (and Its Own Problems)

Personally I'm a big fan of Thunderbolt. It's a revolutionary technology that unlocked many things that aren't possible in the past. One of them is external GPU (eGPU). People typically use eGPU to improve gaming or rendering performance. For me, eGPU seems like a good shot to reduce dGPU temp as it offloads dGPU's work and basically lets it run idle.

I don't have any first-hand experience with eGPU. Based on what I read online, it does improve overall system graphics performance at a small performance penalty. However based on the actual setup there can be quirky behaviors. At this point, I don't have a better solution than eGPU without replacing the laptop entirely. So eGPU we go! In the worst case, I'll just return the components so there's nothing to lose.

### Choice of Components

There are 2 components you need for an eGPU system: 1) an enclosure that provides Thunderbolt connectivity as well as power delivery to GPU and 2) a GPU card. Apart from Apple's [official documentation](https://support.apple.com/en-us/HT208544), [this article](https://egpu.io/setup-guide-external-graphics-card-mac/) on [eGPU.io](http://egpu.io) has lots of useful information about setting up eGPU for Mac. After some research I settled on the Razer Core X enclosure and Sapphire RX 580 Pulse GPU.

*Why I chose Razer Core X?* It's recommended by both Apple and eGPU.io. It has a 650W PSU which supports 100W power delivery to MacBook and delivers 550W max to the GPU (i.e. basically any single GPU you can buy). In the Core X lineup there's a Chroma version which also has USB ports and an Ethernet port (basically a Thunderbolt dock). It also features a 750W PSU (read: overkill) and RGB lighting. Since I already has the TS3+ I don't need these extra features (especially RGB). Core X is enough.

*Why I chose Sapphire RX 580 Pulse?* First, Macs only natively support AMD graphics card so we need to go with AMD. Next, we need to choose from RX 400 series, RX 500 series, RX Vega series, or RX 5000 series. RX 400 series cards are too old and mostly not available anymore. I chose RX 580 because it's cheap and performant enough. Vega and 5000 series GPUs are just unnecessarily powerful for my use case. RX 570 should also do the job but the price difference is just too small compared to RX 580 so I just went with the slightly more powerful one. Lastly, I chose Sapphire because they are known to be the largest supplier of AMD graphics cards and you cannot go wrong with their products.

### Lesson Learned: Cable Matters

Since RX 580 only has DisplayPort and HDMI outputs, I need to find a proper cable to connect to my LG UltraFine monitor, which only has USB-C ports. I have a USB-C to DisplayPort cable I used to use to connect another external monitor to my MacBook. Physically it does fit but this time I'm going to use it the other way around (USB-C end will connect to monitor instead of GPU). It turns out this is not the play as this specific cable is uni-directional so the USB-C end must be connected to the source. **By connecting the cable in the wrong way, my LG monitor doesn't turn on anymore and I guess I fried it.** The LG UltraFine line is specifically designed for Macs so it's possible you have to connect it to a Mac. It's also possible that a proper cable would do the job. Either way I will never find out.

### Second Time Is the Charm

Since I'm determined that I cannot live without an external monitor, I got a replacement monitor – Dell U2720Q, which has DP input so it's impossible to mess up with the cables this time.

**It worked instantly once have everything connected, as Apple advertised.** Not only the GPU is delivering 4K@60Hz video to the external monitor, but also I see a massive temperature drop on CPU and dGPU. To put things into perspective, when system is in idle state:

- Before: CPU and dGPU temps both > 60°C, fan speed > 4000RPM, noise level > 47dBA (reads: jet engine)
- After: CPU temp 45°C, dGPU temp 35°C, fan speed < 1800RPM, noise level < 40dBA (reads: total silence)

The *best* thing is, even when CPU is occasionally under heavy load and CPU temp goes up to 80°C, the fan still spins under 2000RPM which is super quiet. I was expecting an improvement but my expectation was nowhere close to this. I was blown away.

### Eliminating the Last Bit of Noise

If this is a fairy tale, it should end here. But in reality there are always some caveats. Although the eGPU greatly improved overall system acoustics, itself adds another source of noise from the PSU fan. The fan noise is not noticeable in day time and, especially when it's put under the desk. However, the PSU fan is always on and I cannot put up with the low humming noise when I try to sleep. Long story short because I don't want to manually turn the eGPU off after work every day, **I replaced the builtin PSU with another that turns off the fan during light load.**

However swapping the PSU didn't fix the problem completely. I noticed that before the PSU swap, when MacBook is sleeping, the power draw of eGPU is about 8W. But now it's about 35W. The new PSU is likely not the problem because it's actually more power efficient than the previous one. What changed? After some digging I found out that it's because Core X is actually charging MacBook during its sleep. I also learned that **when multiple ThunderBolt devices with power delivery capability are connected, MacBook always prefer the one that delivers the most power**. In my case, I have 2 TB3 devices connected to the MacBook: TS3+ delivers 85W power and Core X delivers 100W power. Therefore MacBook prefers to draw power from Core X. But why did the behavior change though? I have no idea. Anyway we got a problem because it turns out the PSU fan does spin when continuous power needs to be delivered despite having zero fan mode. (I later found out that the fan noise is unnoticeable but at that point I was so obsessed with eliminating PSU fan noise I had to find a solution.)

The first thing I tried to is to limit the wattage from Core X by using a Thunderbolt cable that delivers less than 85W ([suggested by users on eGPU.io](https://egpu.io/forums/psu-cables/macbook-pro-disable-power-delivery/)). However, I just couldn't find such a cable, even for the passive ones. I was so close to give up, accept the fate to have to manually turn off the eGPU every day and laugh at myself for spending so much money and time on modding the Core X. But then, **a light bulb lighted up in my head: what about daisy chain?** To be more concrete, ThunderBolt supports daisy chaining so you can connect TB device A to TB device B and connect B to the host. To the host both devices are connected (technically they form a Thunderbolt device tree). A quick search led me, again, to eGPU.io where a user reported [successful daisy chaining an eGPU](https://egpu.io/forums/thunderbolt-enclosures/success-chaining-caldigit-ts3-plus-dock-egpu-with-three-4k-60hz-displays/). That's a good sign but the user didn't mention the power delivery perspective. If Thunderbolt is smart enough, it could also pass more power from the downstream to the host. But does it? There's only one way to find out. So I unplugged Core X from my MacBook and connected it to TS3+. Bingo! The external monitor still works and System Information reports an 85W power source. I just cannot appreciate enough how versatile a technology Thunderbolt is.

### Other Quirky Things

While it's great that the eGPU can be plug-and-played and works well most of the time, there are a few quirky behaviors I noticed.

- After normal shutdown/restart, login will get stuck if eGPU is connected. A force restart fixes the problem. Users [reported the same issue](https://egpu.io/forums/mac-setup/mojave-10-14-3-update-cant-login-to-osx-when-egpu-is-plugged-in/) on eGPU.io.
- The system can behave weirdly if eGPU is disconnected and connected again. The only reliable way to fix this is to reboot the system (only attach the eGPU after reboot obviously as otherwise you'll run into the issue mentioned above).
- The GPU allocation reporting in Activity Monitor seems bugged. When eGPU is plugged in, most of the processes are shown as being accelerated by iGPU, instead of eGPU, even after [preference](https://support.apple.com/en-us/HT208544#apps) is set. Sometimes a single app is shown to be accelerated by multiple GPUs at the same time. Sometimes dGPU is obviously not used (because of observed low temp) but Activity Monitor still shows it's accelerating random apps. I learned not to care about the reporting too much.
- Core X will go to sleep state when MacBook enters sleep. This is great because power consumption of the eGPU is much lower in sleep (~8W vs. ~45W). However, sometimes MacBook doesn't enter sleep properly, which can be verified by eGPU power consumption (if you have a power meter). When this happens, you can run this command `pmset -g assertions` to check what's preventing the Mac from sleeping. In my case, Intel Power Gadget is often the culprit.

## Conclusion

So, is the eGPU worth it? It fixes the major noise problem I had but the journey to get everything to the ideal state was long. Honestly, had I not switched to 16" MacBook, I wouldn't have all these problems in the first place. Now that the problem is identified, I would stick with my 13" MacBook but use two external monitors instead (i.e. keep my office setup).

### The COVID Impact Is Real

Lots of PC components are very hard to come by at regular price these days, not only because of reduced supply but also thanks to the unprecedented demand caused by people working from home and students doing remote learning.

For the components I used here, the Core X and RX 580 were bought at regular price. But I had to pay additional money for the Dell monitor and EVGA PSU. A Corsair PSU was my first choice but they are so sought after that it's impossible to buy one without paying 50% more (sometimes even double the MSRP price).

The shipping speed is slower too.

### Final Thoughts

Thermal management inside a compact chassis can be complicated. So many components other than CPU and GPU (e.g. VRM, thunderbolt) can overheat and reduce system performance and longevity. The macOS's default fan speed setting probably takes a lot of factors into consideration. That's why the fan can go crazy even if CPU and GPU temps look okay on the surface. In that sense it works pretty well, at the sacrifice of acoustics of course.

And leading up from that it seems you can no longer treat a MacBook as a black box that "just works". At one time I had to keep activity monitor and Intel Power Gadget open all the time so I have visibility of what's going on. I wanted to blame Intel for the 14nm+++++ CPU but after all the research I think Apple themselves are to blame for designing an insufficient thermal solution. That said, I very much look forward to the ARM-based Macs. I will have no problem buying one even if the performance is not as good as Intel Mac's as long as the thermals are significantly improved.
