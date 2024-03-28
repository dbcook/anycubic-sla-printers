# Info and configurations for Anycubic resin SLA printers

The printer config code in the [prusaSlicer github repo](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini)
is way behind on adding the newer and very much improved Anycubic SLA resin printers.

I am very interested in having proper support for the Anycubic Photon Mono M5s printer, which has good build volume, very high
resolution, and high print speeds compared to most existing printers (March 2024).  However there have been no commits to the
prusaSlicer Anycubic.ini file for half a year.

## Output File Formats

Here is a listing of the output file formats for each existing Anycubic printer [source](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini).  This list doesn't readily appear in a search because it's presented as a graphic.

|  Model          | File Format
| -------         | ------------
| Photon          | .photon / .pws
| Photon S        | .photons / .pws
| Photon 0        | .pw0
| Photon X        | .pwx

