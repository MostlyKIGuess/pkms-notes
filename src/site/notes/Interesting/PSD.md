---
{"dg-publish":true,"permalink":"/interesting/psd/"}
---

- The PSD _is a property of a **random** process_. I.e., the noise at the input of an oscilloscope can have a PSD, even if the observed noise signal is different each time you look.
- You can only Fourier-transform things you know. So, you can transform an _observation_ of noise (a so-called _realization of the random process_), but you cannot Fourier-transform "the noise" (as that has uncountably infinitely many possibilities to look like, and on top of that, goes for infinitely long)



Now you think about that and realize that these two things should really better "converge" to the same thing e.g. when you observe for very long, because otherwise, the PSD telling us how much power is in which frequency ranges would be different than the power we observe (via the Fourier transform's magnitude square) in the same regions. And that would mean at least one of them would not describe reality. Huh!

First realization is that if you Fourier transform a very short snippet of noise signal, well, that probably isn't very good at representing the actual power spectrum. Noise is random, there's no guarantee you got a "representative" piece of it in that short time.

So, there's a theorem that links these two: It's the Wiener-Khinchin Theorem.

> From that theorem follows that the PSD calculated from the autocorrelation function (so, expectation of (conjugate) product of integrated signal with time-shifted version of itself; a random property) agrees with the expectation of the Fourier transform of realizations of that process, **under conditions**.

> And these conditions include that the random process be wide-sense stationary (WSS). That means it has a finite expectation that's always the same, for every point in time, and that if you find the autocovariance function, it only depends on the time difference, and not on the absolute time you used to compare the random signal to itself.

Sounds a bit complicated, but really mostly implies that the random signal behaves "randomly, but the same randomly" over all times (and you can be stricter than just looking at how it behaves in autocovariance, hence the "wide-sense").

### Sources:
https://dsp.stackexchange.com/questions/95885/what-information-can-i-obtain-from-power-spectrum-density-psd-that-i-cant-obt/95889#95889
