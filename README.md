"""
Smart Fruit Quality Inspector v2  (OpenCV + NumPy only)

What changed compared with v1
-----------------------------
1. Fruit segmentation no longer uses a fixed "saturation > 30" rule. The
   background colour is learned from the border of the box (or from a
   calibration shot, key 'b') and the fruit is everything that differs from it.
   Lightness is down-weighted so soft shadows are not mistaken for fruit.
2. Defects are found RELATIVE TO THE FRUIT ITSELF, not with one fixed brown HSV
   range:
     A. local darkness  (pixel much darker than the smooth shading around it)
     B. very dark pixels compared with the fruit's median brightness
     C. colour shift   (far from the fruit's median Lab colour AND darker)
     D. mould          (green / blue-green patches on non-green fruit)
3. Specular glare is masked out, the fruit edge is eroded (edges are naturally
   darker) and stems/calyx at the top are ignored.
4. Per-fruit profiles (thresholds differ: banana sugar spots are normal, an
   apple with 5 % rot is not).  Auto-detect by colour/shape, or press 'f'.
5. Results are smoothed over several frames, so the verdict does not flicker.
6. Simple white-balance correction from the background + lighting warnings.

Controls
--------
 d : toggle demo mode          n : next demo sample
 f : cycle fruit profile (AUTO -> each fruit)
 b : calibrate background (box EMPTY!)   c : clear calibration
 + / - : more / less sensitive           s : save snapshot
 q : quit
"""