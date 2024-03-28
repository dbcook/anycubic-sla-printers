# Info and configurations for Anycubic resin SLA printers

The printer config code in the [prusaSlicer github repo](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini)
is way behind on adding the newer and very much improved Anycubic SLA resin printers.

I am very interested in having proper support for the Anycubic Photon Mono M5s printer, which has good build volume, very high
resolution, and high print speeds compared to most existing printers (March 2024).  However there have been no commits to the
prusaSlicer Anycubic.ini file for half a year.

## Anycubic SLA Printer Output File Formats

Here is a listing of the output file formats for each existing Anycubic printer [source](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini).  This list doesn't readily appear in a search because it's presented there as a graphic.

These file formats are all proprietary and are only natively generated by the Anycubic slicer,
which has many problems.  Based on prusaSlicer issue history, there is now some support within
prusaSlicer for some of these formats, but not for any of the recent printer models.

[UVTools](https://github.com/sn4k3/UVtools) knows how to convert Prusa .sl1 files to nearly all
of the Anycubic formats including the latest .pm5 and .pm5s, and some Reddit posts recommend
using it.  This will be my first try at getting output from Prusa to print on my M5s.  See
the workflow outlined in [this reddit post](https://www.reddit.com/r/AnyCubicPhotonMonoX/comments/uvyi3q/anyone_use_prusaslicer_for_anycubic_photon/)

|  Model                 | File Format      | PrusaSlicer
| -------                | ------------     | ------
| Photon                 | .photon / .pws   |
| Photon S               | .photons / .pws  |
| Photon 0               | .pw0             |
| Photon X               | .pwx             |
| Photon Mono            | .pwmo            |
| Photon Mono 4K         | .pwma            |
| Photon Mono SE         | .pwms            |
| Photon Mono X          | .pwmx            |
| Photon Mono X2         | .pmx2            |
| Photon Mono SQ         | .pmsq            |
| Photon Ultra           | .dlp             |
| Photon D2              | .dl2p            |
| Photon Mono X 6K       | .pwmb            |
| Photon M3              | .pm3             |
| Photon M3 Plus         | .pwmb            |
| Photon M3 Max          | .pm3m            |
| Photon M3 Premium      | .pm3r            |
| Photon Mono 2          | .pm3n            |
| Photon Mono X 6Ks      | .px6s            |
| Photon Mono M5         | .pm5             |
| Photon Mono M5S        | .pm5s            |
