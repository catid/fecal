# FEC-AL
## Forward Error Correction at the Application Layer in C

FEC-AL is a simple, portable, fast library for Forward Error Correction.
From a block of equally sized original data pieces, it generates recovery
symbols that can be used to recover lost original data.

* It requires that data pieces are all a fixed size.
* It can take as input an unlimited number of input blocks.
* It can generate an unlimited stream of recovery symbols used for decoding.
* It has a small (about 1%) chance of failing to recover, so it is not an MDS code.

The main limitation of the software is that it gets slower as O(N^^2) in
the number of inputs or outputs.  In trade, the encoder overhead is unusually
low, and the decoder is extremely efficient when recovering from a small number
of losses.  It may be the best choice based on practical evaluation.

FEC-AL is a block codec derived from the [Siamese](https://github.com/catid/siamese) streaming FEC library.


#### Why fecal matters:

It supports an unlimited number of inputs and outputs, similar to a Fountain Code,
but it is designed as a Convolutional Code.  This means that it does not perform
well with a large number of losses.  It is faster than existing erasure correction
code (ECC) software when the loss count is expected to be small.


#### Encoder API:

```
#include "fecal.h"
```

For full documentation please read `fecal.h`.

+ `fecal_init()` : Initialize library.
+ `fecal_encoder_create()`: Create encoder object.
+ `fecal_encode()`: Encode a recovery symbol.
+ `fecal_free()`: Free encoder object.


#### Decoder API:

```
#include "fecal.h"
```

For full documentation please read `fecal.h`.

+ `fecal_init()` : Initialize library.
+ `fecal_decoder_create()`: Create a decoder object.
+ `fecal_decoder_add_original()`: Add original data to the decoder.
+ `fecal_decoder_add_recovery()`: Add recovery data to the decoder.
+ `fecal_decode()`: Attempt to decode with what has been added so far, returning recovered data.
+ `fecal_decoder_get()`: Read back original data after decode.
+ `fecal_free()`: Free decoder object.


#### Benchmarks:

For random losses in 2 MB of data split into 1000 equal-sized 2000 byte pieces:

```
Encoder(2 MB in 1000 pieces, 1 losses): Input=6968.64 MB/s, Output=6.96864 MB/s, (Encode create: 7225.69 MB/s)
Decoder(2 MB in 1000 pieces, 1 losses): Input=9083.06 MB/s, Output=9.08307 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 2 losses): Input=7181.33 MB/s, Output=14.5063 MB/s, (Encode create: 7663.72 MB/s)
Decoder(2 MB in 1000 pieces, 2 losses): Input=7365.13 MB/s, Output=14.7303 MB/s, (Overhead = 0.02 pieces)

Encoder(2 MB in 1000 pieces, 3 losses): Input=6805.5 MB/s, Output=20.4165 MB/s, (Encode create: 7526.72 MB/s)
Decoder(2 MB in 1000 pieces, 3 losses): Input=6312.93 MB/s, Output=18.9388 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 4 losses): Input=6751.28 MB/s, Output=27.0726 MB/s, (Encode create: 7645.84 MB/s)
Decoder(2 MB in 1000 pieces, 4 losses): Input=6387.12 MB/s, Output=25.5485 MB/s, (Overhead = 0.0100002 pieces)

Encoder(2 MB in 1000 pieces, 5 losses): Input=6502.16 MB/s, Output=32.5108 MB/s, (Encode create: 7645.55 MB/s)
Decoder(2 MB in 1000 pieces, 5 losses): Input=5982.11 MB/s, Output=29.9106 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 6 losses): Input=6014.13 MB/s, Output=36.3855 MB/s, (Encode create: 7238.51 MB/s)
Decoder(2 MB in 1000 pieces, 6 losses): Input=5520.74 MB/s, Output=33.1245 MB/s, (Overhead = 0.0500002 pieces)

Encoder(2 MB in 1000 pieces, 7 losses): Input=6284.56 MB/s, Output=44.1176 MB/s, (Encode create: 7764.88 MB/s)
Decoder(2 MB in 1000 pieces, 7 losses): Input=5601.61 MB/s, Output=39.2113 MB/s, (Overhead = 0.02 pieces)

Encoder(2 MB in 1000 pieces, 8 losses): Input=5854.97 MB/s, Output=46.8398 MB/s, (Encode create: 7388.25 MB/s)
Decoder(2 MB in 1000 pieces, 8 losses): Input=5492.54 MB/s, Output=43.9403 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 9 losses): Input=5843.34 MB/s, Output=52.6485 MB/s, (Encode create: 7645.84 MB/s)
Decoder(2 MB in 1000 pieces, 9 losses): Input=5221.11 MB/s, Output=46.99 MB/s, (Overhead = 0.0100002 pieces)

Encoder(2 MB in 1000 pieces, 10 losses): Input=5728.53 MB/s, Output=57.3998 MB/s, (Encode create: 7610.06 MB/s)
Decoder(2 MB in 1000 pieces, 10 losses): Input=5172.24 MB/s, Output=51.7224 MB/s, (Overhead = 0.0200005 pieces)

Encoder(2 MB in 1000 pieces, 11 losses): Input=5590.65 MB/s, Output=61.4972 MB/s, (Encode create: 7667.83 MB/s)
Decoder(2 MB in 1000 pieces, 11 losses): Input=5012.53 MB/s, Output=55.1378 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 13 losses): Input=5382.13 MB/s, Output=70.0753 MB/s, (Encode create: 7687.28 MB/s)
Decoder(2 MB in 1000 pieces, 13 losses): Input=4790.53 MB/s, Output=62.2769 MB/s, (Overhead = 0.0200005 pieces)

Encoder(2 MB in 1000 pieces, 15 losses): Input=5065.47 MB/s, Output=76.0327 MB/s, (Encode create: 7556.01 MB/s)
Decoder(2 MB in 1000 pieces, 15 losses): Input=4490.45 MB/s, Output=67.3567 MB/s, (Overhead = 0.0100002 pieces)

Encoder(2 MB in 1000 pieces, 16 losses): Input=4874.6 MB/s, Output=77.9936 MB/s, (Encode create: 7390.71 MB/s)
Decoder(2 MB in 1000 pieces, 16 losses): Input=4279.45 MB/s, Output=68.4712 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 18 losses): Input=4707.99 MB/s, Output=84.7438 MB/s, (Encode create: 7515.69 MB/s)
Decoder(2 MB in 1000 pieces, 18 losses): Input=4008.9 MB/s, Output=72.1602 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 20 losses): Input=4619.15 MB/s, Output=92.4754 MB/s, (Encode create: 7679.31 MB/s)
Decoder(2 MB in 1000 pieces, 20 losses): Input=3858.4 MB/s, Output=77.1679 MB/s, (Overhead = 0.0200005 pieces)

Encoder(2 MB in 1000 pieces, 25 losses): Input=4176.24 MB/s, Output=104.448 MB/s, (Encode create: 7576.33 MB/s)
Decoder(2 MB in 1000 pieces, 25 losses): Input=3374.22 MB/s, Output=84.3554 MB/s, (Overhead = 0.0100002 pieces)

Encoder(2 MB in 1000 pieces, 30 losses): Input=3731.27 MB/s, Output=111.976 MB/s, (Encode create: 7418.12 MB/s)
Decoder(2 MB in 1000 pieces, 30 losses): Input=2950.2 MB/s, Output=88.506 MB/s, (Overhead = 0.0100002 pieces)

Encoder(2 MB in 1000 pieces, 35 losses): Input=3542.46 MB/s, Output=124.021 MB/s, (Encode create: 7610.64 MB/s)
Decoder(2 MB in 1000 pieces, 35 losses): Input=2702.99 MB/s, Output=94.6048 MB/s, (Overhead = 0.00999832 pieces)

Encoder(2 MB in 1000 pieces, 40 losses): Input=3365.53 MB/s, Output=134.621 MB/s, (Encode create: 7846.52 MB/s)
Decoder(2 MB in 1000 pieces, 40 losses): Input=2410.42 MB/s, Output=96.4169 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 50 losses): Input=2658.13 MB/s, Output=132.933 MB/s, (Encode create: 6889.42 MB/s)
Decoder(2 MB in 1000 pieces, 50 losses): Input=1917.88 MB/s, Output=95.8938 MB/s, (Overhead = 0.00999832 pieces)

Encoder(2 MB in 1000 pieces, 60 losses): Input=2573.04 MB/s, Output=154.408 MB/s, (Encode create: 7578.92 MB/s)
Decoder(2 MB in 1000 pieces, 60 losses): Input=1612.62 MB/s, Output=96.757 MB/s, (Overhead = 0.00999832 pieces)

Encoder(2 MB in 1000 pieces, 70 losses): Input=2141.83 MB/s, Output=149.95 MB/s, (Encode create: 6861.77 MB/s)
Decoder(2 MB in 1000 pieces, 70 losses): Input=1325.65 MB/s, Output=92.7957 MB/s, (Overhead = 0.0100021 pieces)

Encoder(2 MB in 1000 pieces, 80 losses): Input=2052.65 MB/s, Output=164.212 MB/s, (Encode create: 7454.34 MB/s)
Decoder(2 MB in 1000 pieces, 80 losses): Input=1112 MB/s, Output=88.9601 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 90 losses): Input=1926.69 MB/s, Output=173.402 MB/s, (Encode create: 7593.01 MB/s)
Decoder(2 MB in 1000 pieces, 90 losses): Input=972.81 MB/s, Output=87.5529 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 100 losses): Input=1814.67 MB/s, Output=181.467 MB/s, (Encode create: 7866.27 MB/s)
Decoder(2 MB in 1000 pieces, 100 losses): Input=861.668 MB/s, Output=86.1668 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 110 losses): Input=1617.09 MB/s, Output=177.88 MB/s, (Encode create: 7514.28 MB/s)
Decoder(2 MB in 1000 pieces, 110 losses): Input=740.198 MB/s, Output=81.4218 MB/s, (Overhead = 0 pieces)

Encoder(2 MB in 1000 pieces, 120 losses): Input=1485.21 MB/s, Output=178.225 MB/s, (Encode create: 7274.05 MB/s)
Decoder(2 MB in 1000 pieces, 120 losses): Input=645.417 MB/s, Output=77.4501 MB/s, (Overhead = 0 pieces)
```


#### Comparisons:

Comparing with `wh256`, which is [Wirehair](https://github.com/catid/wirehair) using the GF256 library instead of the old library so it runs faster:

For the same data sizes and about 100 losses:

```
>> wirehair_encode(N = 1000) in 2174.33 usec, 919.825 MB/s after 98.992 avg losses
<< wirehair_decode(N = 1000) average overhead = 0.023 blocks, average reconstruct time = 1519.61 usec, 1316.13 MB/s
```

Wirehair is asymptotically O(N) in speed, but for smaller input or output data it can be beaten by other codecs.
In this case the Fecal encoder is twice as fast as Wirehair.  Wirehair is almost twice as fast to decode,
but it takes the same time regardless of the number of losses, so Fecal is much faster for small loss counts.

For the same data sizes and about 30 losses:

```
>> wirehair_encode(N = 1000) in 2281.65 usec, 876.559 MB/s after 30.931 avg losses
<< wirehair_decode(N = 1000) average overhead = 0.02 blocks, average reconstruct time = 1462.48 usec, 1367.54 MB/s
```

Now Wirehair is 4x slower to encode and 2x slower to decode.  There is definitely a large, useful region of operation
where the Fecal algorithm is preferred.


#### Smaller input benchmark:

For random losses in 0.2 MB of data split into 100 equal-sized 2000 byte pieces:

```
Encoder(0.2 MB in 100 pieces, 1 losses): Input=5899.71 MB/s, Output=58.9971 MB/s, (Encode create: 6251.95 MB/s)
Decoder(0.2 MB in 100 pieces, 1 losses): Input=8257.64 MB/s, Output=82.5764 MB/s, (Overhead = 0 pieces)

Encoder(0.2 MB in 100 pieces, 2 losses): Input=6040.47 MB/s, Output=122.018 MB/s, (Encode create: 6680.03 MB/s)
Decoder(0.2 MB in 100 pieces, 2 losses): Input=6572.46 MB/s, Output=131.449 MB/s, (Overhead = 0.02 pieces)

Encoder(0.2 MB in 100 pieces, 3 losses): Input=5474.95 MB/s, Output=165.344 MB/s, (Encode create: 6391.82 MB/s)
Decoder(0.2 MB in 100 pieces, 3 losses): Input=5274.26 MB/s, Output=158.228 MB/s, (Overhead = 0.02 pieces)

Encoder(0.2 MB in 100 pieces, 4 losses): Input=5298.01 MB/s, Output=212.98 MB/s, (Encode create: 6504.06 MB/s)
Decoder(0.2 MB in 100 pieces, 4 losses): Input=5055.61 MB/s, Output=202.224 MB/s, (Overhead = 0.02 pieces)

Encoder(0.2 MB in 100 pieces, 5 losses): Input=5289.61 MB/s, Output=264.48 MB/s, (Encode create: 6768.19 MB/s)
Decoder(0.2 MB in 100 pieces, 5 losses): Input=4785.83 MB/s, Output=239.292 MB/s, (Overhead = 0 pieces)

Encoder(0.2 MB in 100 pieces, 6 losses): Input=4945.6 MB/s, Output=297.23 MB/s, (Encode create: 6648.94 MB/s)
Decoder(0.2 MB in 100 pieces, 6 losses): Input=4356.35 MB/s, Output=261.381 MB/s, (Overhead = 0.0100002 pieces)

Encoder(0.2 MB in 100 pieces, 7 losses): Input=4621.07 MB/s, Output=324.399 MB/s, (Encode create: 6466.21 MB/s)
Decoder(0.2 MB in 100 pieces, 7 losses): Input=4024.95 MB/s, Output=281.747 MB/s, (Overhead = 0.02 pieces)

Encoder(0.2 MB in 100 pieces, 8 losses): Input=4338.4 MB/s, Output=347.072 MB/s, (Encode create: 6287.33 MB/s)
Decoder(0.2 MB in 100 pieces, 8 losses): Input=3762.94 MB/s, Output=301.035 MB/s, (Overhead = 0 pieces)

Encoder(0.2 MB in 100 pieces, 9 losses): Input=4346.88 MB/s, Output=391.654 MB/s, (Encode create: 6548.79 MB/s)
Decoder(0.2 MB in 100 pieces, 9 losses): Input=3592.6 MB/s, Output=323.334 MB/s, (Overhead = 0.0100002 pieces)

Encoder(0.2 MB in 100 pieces, 10 losses): Input=4168.4 MB/s, Output=417.257 MB/s, (Encode create: 6553.08 MB/s)
Decoder(0.2 MB in 100 pieces, 10 losses): Input=3413.55 MB/s, Output=341.355 MB/s, (Overhead = 0.0100002 pieces)
```


#### Comparisons:

Comparing with `cm256`, which is a Cauchy Reed-Solomon erasure code library using GF256:

```
Encoder: 2000 bytes k = 100 m = 1 : 7.69775 usec, 25981.6 MBps
Decoder: 2000 bytes k = 100 m = 1 : 15.0289 usec, 13307.7 MBps
Encoder: 2000 bytes k = 100 m = 2 : 37.7556 usec, 5297.23 MBps
Decoder: 2000 bytes k = 100 m = 2 : 36.2894 usec, 5511.25 MBps
Encoder: 2000 bytes k = 100 m = 3 : 69.2797 usec, 2886.85 MBps
Decoder: 2000 bytes k = 100 m = 3 : 43.9871 usec, 4546.78 MBps
Encoder: 2000 bytes k = 100 m = 4 : 56.8167 usec, 3520.09 MBps
Decoder: 2000 bytes k = 100 m = 4 : 74.4116 usec, 2687.75 MBps
Encoder: 2000 bytes k = 100 m = 5 : 107.402 usec, 1862.16 MBps
Decoder: 2000 bytes k = 100 m = 5 : 102.637 usec, 1948.62 MBps
Encoder: 2000 bytes k = 100 m = 6 : 271.987 usec, 735.329 MBps
Decoder: 2000 bytes k = 100 m = 6 : 300.945 usec, 664.573 MBps
Encoder: 2000 bytes k = 100 m = 7 : 371.691 usec, 538.081 MBps
Decoder: 2000 bytes k = 100 m = 7 : 336.135 usec, 594.999 MBps
Encoder: 2000 bytes k = 100 m = 8 : 244.129 usec, 819.241 MBps
Decoder: 2000 bytes k = 100 m = 8 : 251.093 usec, 796.517 MBps
Encoder: 2000 bytes k = 100 m = 9 : 282.251 usec, 708.59 MBps
Decoder: 2000 bytes k = 100 m = 9 : 282.984 usec, 706.754 MBps
Encoder: 2000 bytes k = 100 m = 10 : 307.543 usec, 650.315 MBps
Decoder: 2000 bytes k = 100 m = 10 : 313.775 usec, 637.4 MBps
```

Fecal is only slower for the special single loss case where `cm256` uses XOR,
in all other cases the new library is much faster.  For 10 losses, it is 6x faster.
Note that `cm256` is also limited to 255 inputs or outputs.


#### How fecal works:

The library uses Siamese Codes for a structured convolutional matrix.
This matrix has a fast matrix-vector product involving mostly XOR operations.
This allows Siamese Codes to encode and decode much faster than other
convolutional codes built on Cauchy or Vandermonde matrices.
Let's call this the Siamese Matrix Structure or something similar.

To produce an output packet, some preprocessing is performed.

The input data is first split into 8 "lanes" where every 8th symbol {e.g. 0, 8, 16, 24, ...} is summed together.
The second "lane" starts from input symbol 1 and contains every 8th symbol after that {e.g. 1, 9, 17, 25, ...}.

For each "lane" there are three running "sums":

+ Sum 0: Simple XOR between all inputs in that lane.
+ Sum 1: Each input is multiplied by a coefficient provided by `GetColumnValue`, and then XORed into the sum.
+ Sum 2: Each input is multiplied by the same coefficient squared, and then XORed into the sum.

This means there are 24 running sums, each with symbol_bytes bytes of data.

When an output is being produced (encoded), two running sums are formed temporarily.  Both are generated
through the same process, and the result of one sum is multiplied by a row coefficient produced by the
`GetRowValue` function and added to the other sum to produce the output.

To produce each of the two sums, a formula is followed.
For each lane, the `GetRowOpcode` function returns which sums should be used.
Sums 0, 1, and 2 are incorporated in based on the function output.
And then 1/16 of the input data are selected at random and XORed into each sum.

The Siamese codec and the Fecal decoder both will compute lane sums only when they are needed.
Since some of the 24 sums (about 50%) are unneeded, the number of operations will vary for each row.

The final random XOR is similar to an LDPC code and allows the recovery properties of the code to perform well
on a larger scale above about 32 input symbols.  The GF(2^^8) multiplies dominate the recovery properties for smaller
losses and input symbols.  The specific code used was selected by experimenting with different parameters until a
desired failure rate was achieved with good performance characteristics.

As a result the Siamese Codes mainly use XORs.  So it can run a lot faster than straight GF(2^^8) multiply-add operations.
Since they are still Convolutional Codes, the Siamese Codes also lend themselves to streaming use case.

When AVX2 and SSSE3 are unavailable, Siamese takes 4x longer to decode
and 2.6x longer to encode.  Encoding requires a lot more simple XOR ops
so it is still pretty fast.  Decoding is usually really quick because
average loss rates are low, but when needed it requires a lot more
GF multiplies requiring table lookups which is slower.


#### Credits

Software by Christopher A. Taylor <mrcatid@gmail.com>, making shit happen.

Please reach out if you need support or would like to collaborate on a project.
