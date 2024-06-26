# Info and Configurations for Anycubic Resin SLA printers

The printer config code in the [prusaSlicer github repo](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini)
is way behind on adding the newer and very much improved Anycubic SLA resin printers.

I am very interested in having proper support for the Anycubic Photon Mono M5s printer, which has good build volume, very high
resolution, and high print speeds compared to most existing printers (March 2024).  However there have been no commits to the
prusaSlicer Anycubic.ini file for half a year.

## Anycubic SLA Printer Output File Formats

Here is a listing of the output file formats for each existing Anycubic printer
[source](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini).
This list doesn't readily appear in a search because it's presented on the Anycubic site as a graphic.

These file formats are all proprietary and are only natively generated by the Anycubic slicer,
which has many problems.  Based on prusaSlicer issue history, there is now some support within
prusaSlicer for some of these formats, but not for any of the recent printer models.

[UVTools](https://github.com/sn4k3/UVtools) knows how to convert Prusa .sl1 files to nearly all
of the Anycubic formats including the latest .pm5 and .pm5s, and some Reddit posts recommend
using it.  This will become my first try at getting output from Prusa to print on my M5s.  See
the workflow outlined in [this reddit post](https://www.reddit.com/r/AnyCubicPhotonMonoX/comments/uvyi3q/anyone_use_prusaslicer_for_anycubic_photon/)

This workflow amounts to:
1. Load your .stl design file (from OnShape, OpenSCAD, etc.) into PrusaSlicer
1. Set up the layout and add supports in PrusaSlicer
  1. How to get printer physical params (build volume etc.) set for the Anycubic printer?
  1. Just change file output extension in Anycubic printer def?
1. Slice and save to a .sl1 file (the format for Prusa's SLA printer)
1. Read the .sl1 with UVTools and convert to the needed Anycubic file type
1. Write the resulting Anycubic file to a USB drive and transfer to the printer.

If I have enough time (unlikely) I may look at splicing some of the UVTools output code into a PR
for PrusaSlicer, assuming the licenses are compatible.


|  Model                 | File Format      | PrusaSlicer
| -------                | ------------     | ------
| Photon                 | .photon / .pws   |
| Photon S               | .photons / .pws  |
| Photon 0               | .pw0             |
| Photon X               | .pwx             |
| Photon Mono            | .pwmo            | Yes
| Photon Mono 4K         | .pwma            |
| Photon Mono SE         | .pwms            | Yes
| Photon Mono X          | .pwmx            | Yes
| Photon Mono X2         | .pmx2            |
| Photon Mono SQ         | .pmsq            |
| Photon Ultra           | .dlp             |
| Photon D2              | .dl2p            |
| Photon Mono X 6K       | .pwmb            | Prusa incorrectly(?) lists pwmx for this profile
| Photon M3              | .pm3             |
| Photon M3 Plus         | .pwmb            |
| Photon M3 Max          | .pm3m            |
| Photon M3 Premium      | .pm3r            |
| Photon Mono 2          | .pm3n            |
| Photon Mono X 6Ks      | .px6s            |
| Photon Mono M5         | .pm5             |
| Photon Mono M5S        | .pm5s            |
| Photon Mono M5s Pro    | .m5sp            | from UVTools source

## Creating a Printer Definition for PrusaSlicer

In order to generate output to send to UVTools we need an accurate printer
definition in PrusaSlicer.  The important parameters for this are given
in the following table

| Parameter          | Value        | Source
| ------             | ------       | ------
| Max print height   | 200 mm       | Anycubic website
| Display width      | 223.78 mm    | Anycubic website
| Display height     | 126.38 mm    | Anycubic website
| X pixels           | 13312        | Anycubic website
| Y pixels           | 5120         | Anycubic website
| Bed shape X        | 223.78 mm    | matches existing defs, == width
| Bed shape Y        | 126.38 mm    | matches existing defs, == height
| Bed origin         | 0,0          | --until proven otherwise--
| Scaling correction | 1,1,1        | no rescaling here, should go in material def
| Elephant foot comp | 0.2 mm       | matches existing defs
| Elephant comp min  | 0.2 mm       | matches existing defs
| Gamma correction   | 1.0          | no correction by default
| Minimum exposure   | 1 sec        | comparison with existing defs, may want 0.5 sec?
| Max exposure       | 120 sec      | ? seems high
| Min. initial exp.  | 1 sec        | matches existing defs
| Max. initial exp.  | 300 sec      | matches existing defs
| Format of archive  | m5sp         | ? maybe not supported, use intermediate format to UVTools?
| SLA output precis. | 0.001 mm     | matches existing defs, should it be finer?

##  How the PrusaSlicer Settings UI Works for SLA Printers

You can always choose any printer settings preset you like from the
main Plater screen, where the dropdown is labeled "Printer".

The chosen printer settings preset determines which printer profiles
(on a confusingly named "print settings" tab in the UI) can be selected
from the "SLA print settings" dropdown in the Plater screen.

Finally the combination of "printer settings" and "print settings"
determines which materials are available in the "SLA material"
dropdown in the UI.  The details for this are on the "Material Settings"
tab.

Due to this massively ambiguous labeling, here is a table giving the
correspondence between Plater screen dropdowns and the detail tabs:

|  Item                   |  Plater Dropdown Name   |  Tab Name
|--------                 |  ----------             |  -------------
| Printer settings preset | Printer                 | Printer Settings
| Print profile           | SLA print settings      | Print Settings
| Material settings       | SLA material            | Material Settings

The tabs are also not in the dependency chain order; seemingly
they are in order of how often you would modify things on that tab.

### Linkage to PrusaSlicer Material Definitions and Print Profiles

The slicer material definitions include the needed initial and primary exposure times,
which are printer, material and print profile (mainly layer height) dependent.

The various linkages needed are implemented via keywords in the Notes sections of
those objects plus filter expressions that understand parameter names in the print profile.

The following PrusaSlicer entities are of interest:

* Printer Settings
* Print profile
* Material Settings

The relationships are shown in the following diagram:

![PrusaSlicer Object Relationships](./prusaSlicer_objects.svg)

Notes for the diagram:

* Material settings specify the following properties
  * Cosmetics, cost, volume. density.  These aren't very relevant to automated selection.
  * Initial layer height
  * Exposure time and initial exposure time.  It's not clear if this overrides
  the layer height in the print profile.
  * Expansion correction XYZ, default [1,1,1]
    * This is the right place to do the shrinkage correction; materials vary

* The printer Settings Notes has keywords like
```
PRINTER_VENDOR_ANYCUBIC
PRINTER_MODEL_PHOTONMONOxxxx
```
These can be used in filter expressions for the material settings so that
the given material is only shown as a choice when a matching printer
vendor and model are specified.

* The Print Profile | Dependencies tab has filter regex field
`Compatible printers condition` that establishes with which printers the
Print Profile is compatible.  An example value is
```
printer_notes=~/.*PHOTONMONOX .*/
```
This means that the Printer Settings | Notes must have something
matching the given regex before the Print Profile entry will be
shown in the list for that printer.

* The `material Settings | Dependencies` tab has filter regex fields 
`Compatible printers condition` and `Compatible print profiles condition`
that are used to establish when the material is available.

* An example for  `Material Settings | Dependencies | Compatible printers condition` is
```
printer_notes=~/.*MONO.*/ and printer_notes=~/.*VENDOR_ANYCUBIC.*/ and printer_notes=~/.*SLA.*/
```
* Note that the above regex refers to Printer Settings | Notes.  This particular example is not very robust as it could generate false positive matches.

* Example `Material Settings | Dependencies | Compatible print profiles condition` is
```
layer_height == 0.05
```
* The above refers to Print Settings | Layers and Parameters | "Layer Height" and means that the material spec is only valid when the layer
height == 0.05.  This might be expected since the required exposure time
could vary with the layer height.

## Printer and Resin Parameters for Mono M5s Pro

The only good place to find these right now is [this wiki page](https://wiki.anycubic.com/en/resin-3d-printer/photon-mono-m5s-pro).
The M5s Pro is still so new that it is not listed in the resin parameter tables for each Anycubic resin.

## Resources

[Reddit Anycubic Printers](https://www.reddit.com/r/AnycubicPhoton/)

Reddit also has some groups for specific printer models but they are thinly subscribed compared to the above.

[UVTools](https://github.com/sn4k3/UVtools)

[prusaSlicer github repo](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini)

[List of Anycubic Output Formats](https://github.com/prusa3d/PrusaSlicer/blob/master/resources/profiles/Anycubic.ini)
