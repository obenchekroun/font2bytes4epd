# font2bytes

Python script to create new fonts for WaveShare epaper / e-ink [EPS32 module]

## Acknowledgments

Based on : [TheHeXstyle's font2byte](https://github.com/theHEXstyle/font2bytes). This project has been largely based on pycasso with a few tweaks for my needs. All credit goes to them for making this awesome project.

## Rational

E-ink display from Waveshare are great, but the provided  library only offers basic fonts types, and only 5 fonts sizes (font8, font12, font16, font20 and font24). This is unfortunately very limiting, especially with their HD screen.

This python script is inspired by the waveshare blogpost [this](https://wavesharejfs.blogspot.com/2018/08/make-new-larger-font-for-waveshare-spi.html). But they do not provide any code to use...
On the other hand the font2bytes from [Dominik Kapusta](https://github.com/ayoy/font2bytes/tree/master) is available but requires C++ compilers.

This is a specific version for my need made in python, adapted from TheHeXstyle project.
At this moment it only recreates ASCII caracters, but you can use any font and specify any size. (just make sure that it will fit your epaper display)


## Requirements

* Python 3
* [Pillow](https://pillow.readthedocs.io/en/stable/index.html#) library  (NOTE: Pillow and PIL cannot co-exist in the same environment. Before installing Pillow, please uninstall PIL.)
* [numpy](https://numpy.org/install/) library


## Use

1. Put any font you want to use (.tff) within the `./fonts` folder

2. Within the `font2bytes.py` :
 - specify a new name for the font to create
 - specify the font name that you want to use (default is roboto-Regular)
 - specify the height and the width of the new font. 

3. run the python script
The python script will generate the new `.cpp` or `.c` file within the `./output` folder with the desired name.

4. Within the waveshare library source folder (`lib/Fonts/` or `Arduino\libraries\esp32-waveshare-epd\src` ) :
 - add the new  `.cpp` or `.c`  font file
 - open the fonts.h and
 - add a new `extern` line with the name of the new font, e.g
``` cpp
extern sFONT FontBold40;
```
 - verify that in the `.c` corresponding file, the variable is defined with the right name, as *extern* only act as a declration and not a definition
 ```cpp
sFONT FontBold40 = {
	Font40_Table,
	22, /* Width */
	36, /* Height */
};

 ```

5. [OPTIONAL] make sure that the defined `MAX_HEIGHT_FONT` and `MAX_WIDTH_FONT` are equal or bigger that the new font size. Update the values if required.

6. Use the new font in your script and enjoy!

``` c
Paint_DrawString_EN(5, 0, "waveshare Electronics", &FontBold40, BLACK, WHITE);
```

## Examples
Within the `./output` folder there are already a couple of `.cpp` files that can be used without running the python code. Just follow the instructions from the 4th step
