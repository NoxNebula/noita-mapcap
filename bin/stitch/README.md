# Stitch

A program to stitch (overlapping) image tiles of pixel-art to one big image.

The program will use median filtering for overlapping images.
That means that all moving object will completely disappear, and only the static pixels will be outputted.
But this has the disadvantage of being slower, and objects that move only a little bit may become blurred.

You can also use this program to remove moving objects from a series of photographs taken with a tripod.
But as this tool is designed for pixel art, it only accepts png as input.

## Source images

The source images need to contain their coordinates in the filename, as this program doesn't align the images:

`%d,%d.png`

example list of files:

``` Shell Session
0,0.png
512,0.png
-512,0.png
512,-512.png
```

## Usage

- Either run the program and follow the interactive prompt.
- Or run the program with parameters:
  - `divide int`
    A downscaling factor. 2 will produce an image with half the side lengths. (default 1)
  - `input string`
    The source path of the image tiles to be stitched. (default "..\\..\\output")
  - `output string`
    The path and filename of the resulting stitched image. (default "output.png")
  - `xmax int`
    Right bound of the output rectangle. This coordinate is not included in the output.
  - `xmin int`
    Left bound of the output rectangle. This coordinate is included in the output.
  - `ymax int`
    Lower bound of the output rectangle. This coordinate is not included in the output.
  - `ymin int`
    Upper bound of the output rectangle. This coordinate is included in the output.
  - `prerender`
    Pre renders the image in RAM before saving. Can speed things up if you have enough RAM.
  - `cleanup float`
    Enables cleanup mode with the given float as threshold. This will **DELETE** images from the input folder; no stitching will be done in this mode. A good value to start with is `0.999`, which deletes images where the sum of the min-max difference of each sub-pixel overlapping with other images is less than 99.9%% of the maximum possible sum of pixel differences.

To output the 100x100 area that is centered at the origin use:

``` Shell Session
./stitch -divide 1 -xmin -50 -xmax 50 -ymin -50 -ymax 50
```

To remove images that would cause artifacts (You should recapture the deleted images afterwards):

``` Shell Session
./stitch -cleanup 0.999
```

To enter the parameters inside of the program:

``` Shell Session
./stitch
```

Example output:

``` Shell Session
Enter downscaling factor:1
Enter input path:..\..\output
2019/11/04 23:53:20 Starting to read tile information at "..\..\output"
2019/11/04 23:53:32 Got 20933 tiles
2019/11/04 23:53:32 Total size of the possible output space is (-25620,-36540)-(25620,36540)
Enter output rectangle (xMin,yMin;xMax,yMax):-25620,-36540;25620,36540
Enter output filename and path:output.png
2019/11/04 23:53:35 Creating output file "output.png"
105 / 571 [--------------->____________________________________________________________________] 18.39% 1 p/s ETA 14m0s
```
