# CD-Assessor
A fast, objective, non-visual test for audio CD (CD-DA) media grading. Intended to be useful when selling and buying CDs. Appearances matter for asset value but playibility is what music lovers care about. No care is given to saving the CD audio - this is not about ripping.

# Overview

This is a placeholder repository for an idea I have to create a tool to objectively assess the play condition of audio CDs, as quickly as possible.

The report will:

- Characterise the CD player device: any optical drive for a computer compatibile with CD-DA. <ins>__CD drive__</ins> will be used to represent the optical drive from here on. Eg vendor, model, firmware revision, relevant drive capabilities like: speed options and error checking. Tools like `cd-drive`
- Characterise the audio CD: read Table Of Contents (TOC)
- Report the results of the read tests


## Obective
Listening to an entire CD play can take over an hour and requires full attention of the listener to make an assessment. There may be environmental distractions like machinery, device notifications, etc. Issues with an audio CD may not be audible.


## Classification
Maybe return a percentage for the overall classification, being a function of 
 - tolerance for non-audible errors (corrected)
   - zero tolerance needed for mint and near-mint
 - uncorrected but inaudible errors (eg 1 sector drop)
 - numbers of errors * size of errors

Most CDs play through without audible issue, so most common result will be 100%
 - may need to scale results to fit common grading standards


Approximations to Discogs Goldmine Grading system, for example
 - Mint, Near-mint: A reproducible (>= 2 passes), completely error free
 - Very Good Plus: A reproducible (>= 2 passes), insignificant/correctible errors that don't affect playback
 - Very Good: less than x errors of y consecutive length
 - etc

See also CD classification attempts on the Discogs forums:
 - https://www.discogs.com/help/forums/topic/214182

# Installation

## Hardware
### Computer
Tested with AMD A10-7870K, 16GB RAM

### Optical Drive
Tested with SATA DVD-RW

## Operating System
Tested with Ubuntu 22.04, amd64

### Dependencies
Try to stick to the still-maintained GNU libcdio based utilities: https://www.gnu.org/software/libcdio/

Ubuntu packages: libcdio-utils, libcdio-paranoia2, cd-paranoia, icedax (formerly known as cdda2wav), wodim

# Characterise drive and disc
## CD Drive
```
wodim dev=/dev/sr0 -prcap
```

```
cd-drive
```
Might need to do some read tests to get actual read speeds

## Audio CD (CDDA or CD-DA)
```
cd-info
```

# Read Tests
## Fastest read test
- Read the disc at the maximum speed and save the data to disk (read1)
- Based on the detected maximum read speed of the CD drive, the length of the disk (from the TOC) and the time it actually takes to read the whole disc, does the read complete within a theoretical expectation?
- If this test fails, repeat with slower speed (read2) and compare read1 and read2

```
cdrdao read-cd --datafile image.bin --device /dev/sr0 --read-raw image.toc
```

## Paranoia reads
```cd-read, cd-paranoia, cdda2wav```

```
time cd-paranoia -v -e -l cd-paranoia1.log -w -X -B 1-2 > cd-paranoia2.log 2>&1
```
## Compare reads
Binary diff?

