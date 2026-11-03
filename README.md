# Notch Listening

Educational self-help page for matching a tinnitus pitch and playing notched noise.

Not a medical device. Not intended to diagnose, treat, cure, or prevent any condition. Not medical advice. Keep the volume low and comfortable, and stop if you feel discomfort.

## Design history

### Oct 2, 2026: the idea (Business Idea chat)

Nick asked for a hard notch that steps through a band, and chose half-octave slices. His reason, in his own words on Nov 2: "my ringing is wider than the half octave so i wanted to hit it all, not just one little slice. And, it doesn't hurt to not listen to frequencies, so why not do it?" His ringing has several close pitches, so one narrow slice would miss part of it.

### Study basis

The tailor-made notched music work by Okamoto et al. and the follow-up studies only set the notch width: about 1 octave (half an octave in some protocols), centered on the tinnitus pitch. The notch stays in one place. Stepping the notch across a band was Nick's own design, not something from the studies.

### Oct 3, 2026: original build

- Step 2 stepped through 5 half-octave slices centered at 1842, 2605, 3684, 5210 and 7368 Hz, 30 seconds each, then looped.
- Bug: the lowpass and highpass filters had Q set to 0.7071, but Web Audio reads Q in dB for those filter types. Each stage resonated, adding bumps of about +7 dB beside each notch. As a result, 3684 Hz was boosted about 6 dB during 4 of the 5 slices, so about 80% of each session.
- Bug: the notch chart was a drawing of an ideal notch, not the real filter response.
- The top section (Step 1) was a band-pass listening aid for finding the pitch. It was never meant to be a notch. Since the Nov 2 morning commit it plays pure tones instead of noise.

### Nov 2, 2026: fix (commit c6ae6d8)

- One fixed notch, one octave wide, from f/sqrt2 to f*sqrt2 (2605 to 5210 Hz at 3684 Hz). It stays in place for the whole session.
- Built as a 2047-tap Kaiser band-stop FIR (beta 8) in a ConvolverNode, with a short crossfade when the pitch changes.
- The chart now draws the real response of the filter being played.
- Measured on the live page: about -116 dB at 3684 Hz, more than 20 dB of cut from 2650 to 5150 Hz, and at most +0.06 dB anywhere else. Nick confirmed it on his phone.

### Why not wider

The notch has to stay narrow and sit right next to the ringing. The idea behind the Okamoto approach is lateral inhibition. The ringing comes from an overexcited area of the hearing system. The notch gives that area no sound, while its neighbors just outside the notch keep getting sound. When the neighbors are active, they calm the ringing area. A notch that is too wide silences the neighbors too, so it stops working.

Decision (Nov 2, 2026): keep the fixed one-octave notch. The old stepping across 5 half-octave slices (about 1550 to 8760 Hz in total) is not coming back, for the same reason.
