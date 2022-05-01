# Confetti [![Generic badge](https://img.shields.io/badge/Difficulty-Easy-green.svg)](https://shields.io/)

## Description

"From the sky, drop like confetti All eyes on me, so V.I.P All of my dreams, from the sky, drop like confetti" - Little Mix
`confetti.png`

Author: cavocado

## Files
```
.
└── confetti.png
```

## Solution

Since the challenge only provided a single png file, it was undoubtedly a steganography challenge. So first, I consulted steghide.

```bash
$ steghide -sf confetti.png
steghide: the file format of the file "confetti.png" is not supported.
```

Since this was a dead-end, I looked at the image's EXIF data, and there was a hint of what to do.

```
$ exiftool confetti.png
...
Warning: [minor] Trailer data after PNG IEND chunk
...
```

There is data that doesn't belong to the image itself. So there might be an additional flag at the end.

```
$ strings confetti.png | grep -e 'flag'

$ strings confetti.png | grep -e 'actf'

```
Another possibility is a hidden binary inside the image. binwalk should reveal it.

```
$ binwalk confetti.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0         0x0         PNG image, 3971 x 2918, 8-bit/color RGBA, non-interlaced
115       0x73        Zlib compressed data, default compression
967339    0xEC2AB     PNG image, 3971 x 2918, 8-bit/color RGBA, non-interlaced
967454    0xEC31E     Zlib compressed data, default compression
1934678   0x1D8556    PNG image, 3971 x 2918, 8-bit/color RGBA, non-interlaced
1934732   0x1D858C    TIFF image data, big-endian, offset of first image directory: 8
3180408   0x308778    PNG image, 3971 x 2918, 8-bit/color RGBA, non-interlaced
3180523   0x3087EB    Zlib compressed data, default compression
```

And indeed, some files don't belong there. However, we can extract them and have a look at them.

```
$ binwalk -D=".*" confetti.png
```

The file `1D8556` is an image containing the flag in the middle.