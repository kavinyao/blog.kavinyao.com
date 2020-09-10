title: Transition from NZXT H400 to H510
date: 2020-09-08

---

I've been using NZXT H400 for over 2 years. It's a very good looking and well-built case despite the price tag. Initially I transferred my then ITX build into it and needless to say lots of space was wasted. When I [switched from Intel to AMD](/posts/goodbye-intel-hello-amd), I took the opportunity and upgraded to an mATX system — the largest form factor H400 supports. mATX is so much better compared to ITX for those who don't need extreme compactness. With the positive experience with mATX I totally see myself getting an ATX build eventually, given that a compact mid-tower case is not much bigger than H400 (in fact H510 is only 12% larger than H400 in terms of volume).

Recently I need to swap out the PSU from my H400 build which presented a perfect opportunity to upgrade the case. To summarize, the reasons I move away from H400 include:

- It only supports mATX motherboards, which are rarely made these days for some reason so the choices are limited and price tags tend to be higher than they should be.
- The tempered glass side panel is mounted with 4 thumb screws and it's not fun to unscrew 4 of them every time you need to take the panel off.
- The PSU bracket does make PSU installation easier but the mechanism is not as reliable. For my case one of the thumb screw got stuck and I had to forcibly pull it out.
- The cable bar is too obtrusive and sometimes gets in the way.

Honestly none of these are deal-breakers but over the time working with this case becomes more and more frustrated. However I'm not throwing it away but instead plan to reuse it for a home server build later.

The reasons I chose H510:

- I still like NZXT's minimalistic and understated aesthetics.
- It supports ATX motherboards.
- The glass side panel installation is screwless — well almost, there's 1 thumb screw in the back to further secure it.
- The cable bar is less obtrusive.
- It's compact and only slightly larger than H400 so I can still keep it on my desk.
- It has a USB 3.1 Gen2-compatible USB-C connector.
- It's cheap. The non-i version only costs $70.

Basically all my problems with H400 are fixed in H510.

The only thing I don't like about H510 is the worse acoustics. This is not a surprise though since [plenty of](https://www.youtube.com/watch?v=7HK5Aulw7YI) [reviews](https://www.youtube.com/watch?v=_ixFt7h8fak) have pointed out this problem. [Some](https://www.youtube.com/watch?v=ApdliGCqtZg) even suggested not using any front in-take fans as they don't help with reducing CPU temp at load (although I found this claim to be true but having front in-take fans will help CPU idle temp). Luckily, this problem is not unsolvable at a small cost of performance. Since I'm not using any hard drives, the only source of noise is the fans. Apart from the sheer loudness at high load, variation of fan speed within a short time window also introduces additional unpleasant sound (especially when some hardware has zero-fan mode). The tunings I did:

- I added 2 front in-take fans which helps bring down temps (and noise) when system is idle.
- For CPU and chassis fans, I set fan speed at a constant value if CPU temperature is below 55°C and a linear increase up towards 90°C.
- For GPU, I lowered boost clock and power target as well as set a [fan curve](/posts/asus-gpu-zero-fan) that effectively disables zero-fan mode.

Things I tried but didn't work out:

- moving down GPU to the second X16 slot
- lower minimum processor state in Windows 10's power plan options

It wasn't an easy process to tune the acoustics since it's largely an art instead of science. Although there are lots of resources online not many of them are actual useful. At one stage I was even considering swapping in an after-market cooler on the GPU and ditching the new case. However I did learn new techniques and the final setup is actually quieter than my H400 build. And I'm pretty satisfied with the result.
