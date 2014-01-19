AFAICS
======

ASCII For Alternatively-previewing Images, Compressed Superiourly.

What's this supposed to be?
---

Plaintext files, while long not used awarely by most user anymore, remain an important form of data storage, be it in form of handwritten source code or auto-generated XML etc.. The lower performance and density is often irrelevant, while the advantages such as `grep`ping for content, use of powerful text editors, and improved compatibility to version-control systems are quite desirable.

However, some types of data cannot adequately be represented by text various reasons, so they will usually be stored directly to binary files. In case of e.g. audio / video streams of large amounts of detector data, there is not much alternative to this if only because of performance issues, but other data may in terms of information content fit well into a nicely-handleable text file, but would simply not make for any actually readable visual appearance. The usual encoding is Base64, even more incomprehensable as a binary file in a hex editor and just a bit less efficient. As far as looks are concerned, this is basically _noise_, without useful information at all. Also for review of diffs and searching for where some data is actually stored, it is no better than tight binary.

One of the "binary data types" is raster images. However, for those there is in fact a text representation, quality-wise questionable but historically in fact quite relevant: **ASCII Art**! It is in some senses the opposite of Base64: very low information content, but very easy to spot what's in it, particularly in a zoomed-out editor view or when quickly scrolling through a VCS diff. (Admittedly, I've never seen ASCII Art in a diff, so this is a bit of a hypothesis...)

The stark contrast between both systems suggests there might be a way to combine the advantages of both: fill the apparantly-unusable-information-gaps in ASCII Art with a tightly-compressed binary representation, retaining as much of the direct viewability as possible.

Easily the most interesting application is actually storing picture data inside text files, so that you get both a low-resolution version any text editor will reliably preview in any context, while most of information (including all colour), necessary for the end application but not necessarily for preview or searching, is only revealed and used when running the actual binary decoder, yielding a much higher-resolved version of the same picture.


How it could work
---

The easiest workable implementation would simply group the printable ASCII character set (or any other one, for that matter: Unicode would allow storing much more binary information in a visibly equivalent text file!) into e.g. 8 "brightness character classes". The ASCII Art would then be implemented in those greyscale levels, leaving at least 3 more bits per char (which of the characters from a given brightness class was used?) for storing arbitrary binary data. For an image of 80 &times; 40 char picture, that's 1.2 kB, enough to store 160 &times; 120 colour photo in acceptable quality, given a sufficiently powerful lossy-compression codec.

More sophisticated tricks could allow enhancing the capacitity: 

- At few distinguished brightness levels, we will use dither anyway. Though that is actual part of the "visible" picture it could still store additional information, since a low "noise" content is basically ignored by our eyes if we don't watch too closely.
- For the special case of storing a picture inside its own ASCII Art representation, that can itself be used as the part of the high-resolution version, leaving only higher-frequency components to be stored in the binary part. Actually though, this might not be worth the effort since the ASCII Art itself has really low information content.
- Using a large Unicode range would give a lot of storage though it might be difficult to find a sufficiently even brightness range.
