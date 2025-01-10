---
layout: post
title:  "IrisCTF 2025 RF Writeups"
date:   2025-01-10 00:00:00 +0000
categories: bananas-in-pyjamas
---
Here are writeups for 3 of the 4 IrisCTF RF challenges I could solve. The first 3 were relatively simple, the last one I couldn't figure out the right way to approach it, and didn't make progress.

# Dotdotdot
![dotdotdot chal desc](/assets/img/dotdotdot_chal.png)
This challenge is a 50pt beginner challenge. Unzipping the tgz, you get an ".iq" file, which I guessed was meant to be a raw IQ dump. Looking at it as a waterfall plot shows this:

![dotdotdot freq](/assets/img/dotdotdot_freq2.png)

Looks like simple morse code. To decode, we can simply strip the noise, then use an AM demodulation, like this:

![dotdotdot solve](/assets/img/dotdotdot_solve.png)

This produces a signal that looks much more like morse code:

![dotdotdot morse out](/assets/img/dotdotdot_morse.png)

Now, to decode the actual morse code we can manually inspect the waveform, or we can treat the signal as an array, apply a threshold to distinguish values as "high" or "low" (0 or 1), and then use run-length encoding to encode the waveform into periods of high or low signal.

From that point, each long run of high signal is a dash, and each short run of high signal is a dot - low signal represents spaces. A tiny bit of manual help toward the end of the signal was required, as some noise meant one character wouldn't decode properly - but this is immediately apparent when viewing the waveform in audacity.

# RFOIP
![rfoip chal desc](/assets/img/rfoip_chal.png)

This challenge is another relatively easy challenge, but was 400+ points when I did it, indicating not many people attempted it.

This turned out to be rather simple - the supplied IP was simply providing a stream of IQ samples, which we could directly interpret as audio. The following gnuradio graph, and a little patience, was enough to get the flag:

![rfoip solve](/assets/img/rfoip_solve.png)

# Sine FM
![sinefm chal desc](/assets/img/sinefm_chal.png)

This challenge was also worth nearly 500 points at the time of solving. This was very similar to RFOIP. Looking at the waterfall diagram seems to indicate it's something frequency modulated:

![sinefm freq desc](/assets/img/sinefm_freq.png)

It turns out it's - unsurprisingly - just FM audio. Simply demodulating it, then a post-process band-pass filter to clean up the audio quality a bit is enough to solve the challenge:

![sinefm solve](/assets/img/sinefm_solve.png)
