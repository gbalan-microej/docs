======
Images
======

MicroUI makes the distinction about three kinds of images:

* the images which can be used by the application without a loading step: class ``Image``,
* the images which requires a loading step before being usable by the application: class ``ResourceImage``,
* the buffered images where the application can draw into: class ``BufferedImage``.

First kind of image requires the Image Engine must be able to use (get, read and draw) an image referenced by its path without any loading step. The *open* step should be very fast: just have to find the image in the application resources list and create an ``Image`` object which targets the resource. No RAM memory to store the image pixels is required: the Image Engine directly uses the resource address (often in FLASH memory). And finally, *closing* step is useless because there is nothing to free (except ``Image`` object itself, via the garbage collector).

Second kind of image requires the Image Engine must be able to use (load, read and draw) an image referenced by its path with or without any loading step. When the image is understable by the Image Engine without any loading step, the image is considered like the first kind of image (fast *open* step, no RAM memory, useless *closing* step). When a loading step is required (dynamic decoding, external resource loading, image format conversion), the *open* state becomes longer and a buffer in RAM is required to store the image pixels. By consequence a *closing* step is required to free the buffer when image becomes useless.

Third kind of image requires, by definition, a buffer to store the image pixels. Image Engine must be able to use (create, read and draw) this kind of image. The *open* state consists to create a buffer. By consequence a *closing* step is required to free the buffer when image becomes useless. Contrary to the other kinds of images, the application will be able to draw into this image.

The Image Engine is composed of:

* An "Image Generator" module, for converting images into a MicroEJ format (known by the Image Engine Core) or into a platform binary format (cannot be used by the Image Engine Core), before runtime (pre-generated images).
* The "Image Loader" module, for loading, converting and closing the images. 
* A set of "Image Decoder" modules, for converting standard image formats into a MicroEJ format (known by the Image Core) at runtime. Each Image Decoder is an additional module of the main module "Image Loader".
* The "Image Core" module, for reading and drawing the images in MicroEJ format.

png -> imagen -> RAW
png -> loader -> pngdecoder -> RAW
bmp -> loader -> bmpdecoder -> RAW
xxx -> loader -> platformdecoder -> RAW
external png -> loader -> pngdecoder -> RAW
external raw ->
raw -> loader -> convert -> raw
etc.

.. graphviz::

   digraph {
         rankdir=LR;
         {
            png [label="png", shape=note]
            jpg [label="jpg", shape=note]
            bmp [label="bmp", shape=note]
            raw [label="raw", shape=note]
            ig [label="Image Generator", shape=rect]
            app [label="Application", shape=rect]
            ll [label="LLUI_Painter.h", shape=signature]
            sa [label="Software Algorithm", shape=rect]
            gpu [label="GPU", shape=rect]
            i [label="", image="images/applist_50.png", shape=plaintext]
         }

         subgraph flow {
            {png, jpg, bmp} -> ig -> raw -> app 
            app -> {ll, sa} 
            ll -> gpu -> i
            sa -> i
         }
   }

Functional Description
======================

.. _section_image_core_process:


.. figure:: images/image_process.*
   :alt: Image Engine Principle
   :width: 80.0%
   :align: center

   Image Engine Principle

Process overview:

1. The user specifies the pre-generated images to embed (see
   :ref:`section_image_generator`) and / or the images to embed as
   regular resources (see :ref:`section_image_runtime`)

2. The files are embedded as resources with the MicroEJ Application. The
   files' data are linked into the FLASH memory.

3. When the MicroEJ Application creates a MicroUI Image object, the
   Image Engine Core loads the image, calling the right sub Image Engine
   module (see :ref:`section_image_generator` and
   :ref:`section_image_runtime`) to decode the specified image.

4. When the MicroEJ Application draws this MicroUI Image on the display
   (or on buffered image), the decoded image data is used, and no more
   decoding is required, so the decoding is done only once.

5. When the MicroUI Image is no longer needed, it is garbage-collected
   by the platform; and the Image Engine Core asks the right sub Image
   Engine module (see :ref:`section_image_generator` and
   :ref:`section_image_runtime`) to free the image working area.

.. toctree::
    :maxdepth: 2

    uiImageRaw
    uiImageGenerator
    uiImageLoader
    uiImageCore




..
   | Copyright 2008-2020, MicroEJ Corp. Content in this space is free 
   for read and redistribute. Except if otherwise stated, modification 
   is subject to MicroEJ Corp prior approval.
   | MicroEJ is a trademark of MicroEJ Corp. All other trademarks and 
   copyrights are the property of their respective owners.
