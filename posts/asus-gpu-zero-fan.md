title: Fix ASUS GPU Zero Fan Turning On and Off
date: 2019-06-11

---

I have an [ASUS GTX 1080 GPU](https://www.asus.com/us/Graphics-Cards/ROG-STRIX-GTX1080-A8G-GAMING/). It has a feature called "Zero Fan", which doesn't turn on GPU fan until the GPU temp reaches a threshold (the default is 50°C I believe). Sounds great isn't it? Well, in reality, it could bring more noise than it reduces. The problem is that, even if you are not running games or 3D rendering, the GPU is still being used. The Windows 10 task manager shows GPU usage by application. At the moment of writing, my GPU has 4-6% utilization, used by Chrome, VSCode, Wallpaper Engine and Desktop Window Manager. This load is pretty tiny but passive cooling is not enough to remove the heat the GPU produces. As a result, the GPU temp slowly creeps up until the zero fan threshold, when GPU fans are turned on for a while. Right after the GPU temp drops below the threshold, the fans are turned off. After a while the GPU temp reaches threshold again, the fans are turned on again...

By all means, the Zero Fan feature works perfectly as advertised. However, when the fans jumps between 0% to 30%+ duty cycle, it produces noticeable sharp noises, especially in quiet environment like my study. It's super disturbing to me when I read things. What makes it worse is even though GPU Tweak II, the ASUS GPU control software, allows you to customize fan curve, it forces a zero fan setting.

How to solve the problem? The answer is simple: keep the fan running at low RPM. You can start with 0%@20°C - 100%@90°C. For my GPU, the temp stabilizes at 34%@42°C. One thing to note that the minimal operating duty cycle is typically around 30% for GPU fans. That means you need to make sure the duty cycle is above 30% at the stable idle temp. Otherwise the software will turn off the fans every once in a while to make the average duty cycle below 30%.
