# Representation of fingerprint images in memory #

Part of [SourceAFIS idea blog](../README.md).

Images are decoded into an array of numbers (see below) from their serialized form
(JPEG, PNG, WSQ, BMP, or any of the standard or raw image formats).
This decoded format is then used for all image processing.

Types of image arrays:

- Two-dimensional array if the programming language supports it.
  Y axis is usually the outer dimension.
- Array of arrays that simulates two-dimensional array described above.
  It may be advantageous for parallelization,
  because every row of the array may be allocated and initialized in different thread.
- One-dimensional array with pixel index calculated as y*w+x or x*h+y
  where x and y are pixel coordinates and w and h are the width and height of the image.

X and Y axes are oriented right and down respectively,
which is the usual way computers represent images.
Y axis may be turned up to use standard right-handed cartesian coordinate math.

Image pixels are encoded as single value (shade of gray).
Color input is reduced to grayscale by averaging color channels
or by picking one channel, perhaps after conversion to HSV, HSL,
or some other color space.

Scalar values inside the array may be of several types:

- Floating-point values (of any precision) are easy to transform, especially multiplicatively,
  free of out-of-range errors, and they preserve maximum accuracy throughout image processing.
- Fixed-point values (of any precision), possibly simulated using integer values,
  may be faster to process and take less memory. Their downside is in loss of accuracy,
  risk of out-of-range errors, and possibly harder development.
- Integers (of any precision) work well for additive math, but multiplicative math
  on integers usually resorts to emulating fixed-point values.
  Their advantage is fast processing even on embedded hardware.
- Booleans (or 1-bit integers) can represent ridges and valleys very explicitly
  and they can also represent continuous fingerprint areas (or masks).

Compared to standard image formats,
where black is a low value (usually 0) and white is a high value (usually 255),
values in decoded fingerprint image array are often inverted
with black represented by higher number than white,
because black ridges are the signal while white color is the background.

Brightness range may be centered on zero, especially after image equalization,
which has the advantage of ridges and valleys being on different sides of zero.
Brightness range may be normalized to 0..1 or -1..1,
which helps ensure that multiplication will keep the values in the same range.

Some parts of the image array may not be used.
This space can be filled with reasonable default values,
perhaps white, average brightness, or average background brightness.
Unused parts of the image may be identified with a mask (1-bit or boolean image).
Algorithms processing the image can either check the mask before read
or they can rely on the default value being acceptable.

Image arrays may be kept off-heap or in GPU memory
for better performance or to enable processing by native code or hardware.
Images may be placed in shared memory area to enable processing
by another process, possibly implemented in different programming language.

Published by Robert Važan on March 11, 2018

