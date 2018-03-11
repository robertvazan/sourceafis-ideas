# Configurable DPI #

Part of [SourceAFIS idea blog](../README.md).

Fingerprints are commonly captured by specialized fingerprint reader devices
that capture images at fixed DPI (dots per inch), commonly around 500dpi,
but DPI can vary from one fingerprint reader to another.
This presents a problem for fingerprint recognition algorithms,
because algorithm parameters (especially distance limits)
or even algorithms themselves might work only at some specific DPI.

One approach to deal with the problem is to normalize DPI of the image on input
by resizing the image using some standard image scaling algorithm.
The standard DPI is chosen as a tradeoff
between fine detail provided by high DPI and fast processing provided by low DPI.
A reasonable choice for standard DPI is 500.
Once the image is resized, the rest of the algorithm can work with the simplifying assumption of fixed DPI.

Box sampling is one good option for image scaling algorithm.
For every output pixel, a box is projected onto the original image.
Values of pixels covered by this box are averaged
with weight proportional to pixel surface covered by the box.
Box sampling reduces to bilinear interpolation for scaling factors higher than 0.5.

Other reasonable choices for image scaling algorithm include bilinear and bicubic scaling.
Nearest neighbor scaling is fast, but it creates artifacts,
which can degrade quality of output from feature extractor.
Many scaling algorithms work better than on average images,
because fingerprint images have continuous shades of gray
and feature extractor performs smoothing of the images, which hides scaling artifacts.

A completely different approach to the DPI problem is to
keep the image unchanged and scale algorithm parameters instead.
The advantage is that fingerprint image doesn't suffer
from artifacts or loss of detail caused by image scaling.
The downside is that algorithms working with unscaled images are more complicated.
Many parameters need to be rounded, which may result in very uneven adjustment of parameters.
Some algorithms are hard to parameterize with DPI.

Even with parameter scaling, conversion of fingerprint features to standard DPI
is still desirable at the end of feature extraction
to allow the matching algorithm to match fingerprints scanned at different DPI.

Published by Robert Važan on March 11, 2018

