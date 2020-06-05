# mosaic

This utility can be used to generate [photo-mosaic](http://en.wikipedia.org/wiki/Photographic_mosaic) images. There are several mosaic libraries.

There is **no cheating** with semi-transparent overlays, tile tinting, or repeating images (by default).

This one is focused on **maximum accuracy** (using scikit's Mean Squared Error). This is a pixel by pixel comparison to produce a best match. Other libraries simplify this by only comparing against a limited (averaged, antialiased) number of pixel from the original sector, or just compare against the histograms. Producing large, highly detailed mosaics takes much time!

As well as an image to use for the photo-mosaic ([most common image formats are supported](http://pillow.readthedocs.org/en/latest/handbook/image-file-formats.html)), you will need a large collection of different images to be used as tiles. The tile images can be any shape or size (the utility will automatically crop and resize them) but for good results you will need a lot of them - a few hundred at least. One convenient way of generating large numbers of tile images is to [extract screenshots from video files](https://trac.ffmpeg.org/wiki/Create%20a%20thumbnail%20image%20every%20X%20seconds%20of%20the%20video) using [ffmpeg](https://www.ffmpeg.org/).

**Prerequisites**
<pre>pip install scikit-image numpy pillow</pre>

**Usage**
```python
create_mosaic(
    subject="/path/to/source/image", 
    target="/path/to/output/image", 
    tile_paths=["/path/to/tile_1" , ... "/path/to/tile_n"],
    tile_ratio=1920/800, # Crop tiles to be height/width ratio
    tile_width=300, # Tile will be scaled
    enlargement=20, # Mosiac will be this times larger than original
    reuse=False, # Should tiles be used multiple times?
    color_mode='L',  # RGB (color) L (greyscale)
) 
```

### Sample: Sean Connery made of screenshots from his Bond films
Sean starred in 7 Bond films. A still every 3 seconds yields about 10,000 images. 2,500 unique stills are matched and used below:
![Sample](https://github.com/dvdtho/mosaic/blob/master/connery_old__3cc96.jpg)


# Image Aspect Crop with Focus (Bonus Feature)
Ability crop an image to the desired perspective at the maximum size available. Centerpoint can be provided to focus the crop to one side or another. 

For example, if we are cropping the left and right sides and the left side is more interesting than the right side:
```python   
from PIL import Image
img = aspect_crop_to_extent(
        img=Image.open("/path/to/image"), 
        target_aspect=1, # width / float(height) -- 1 is square
        centerpoint=(0,0), # (width, height) -- we will focus on the left, and crop from the right
)
```

------------
Changed from original project (https://github.com/codebox/mosaic):  
*   ability to not reuse tiles 
*   increased quality of match by comparing against whole tile images 
*   increased quality of mosaic by starting with the center instead of top left
*   ability to input color mode

Tested with:
*   Python 3.6.5
*   numpy==1.14.4
*   Pillow==5.1.0
*   scikit-image==0.14.1


