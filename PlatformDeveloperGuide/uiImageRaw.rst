.. _section_image_raw:

============
Image Format
============

The Image Engine makes the distinction between the `input formats` (how an image is encoded) and the `output formats` (how the image is used by the platform and/or the Image Core).The Image Engine manages several standard formats in input: PNG, JPEG, BMP etc. In addition, an input format may be custom (platform dependant, unsupported image format by default). It manages two formats in output: the MicroEJ format (known by the Image Core) and the binary format.

Each Image engine module can manage one or several input formats. However the Image Core manages only the MicroEJ format. The binary output format is fully platform dependant and can be used to encode some images which are not usable by MicroUI standard API.

The MicroEJ format is the only binary format known by the Image Core. It is constitued by a header and by the image pixels (in a specific order and with a specific encoding). Several encodings are available. An encoding can be standard or platform dependant (custom).

.. _section_image_standard_raw:

MicroEJ Format: Standard
========================

Several MicroEJ format encodings are available. Some encodings may be directly managed by the display driver. Refers to the platform specification to retrieve the list of better formats.

Advantages:

* The pixels layout and bits format are standard, so it is easy to manipulate these images on the C-side.
* Drawing an image is very fast when the display driver recognizes the format (with or without transparency).
* Supports or not the alpha encoding: select the better format according to the image to encode.

Disadvantages:

* No compression: the image size in bytes is proportional to the number of pixels, the transparency, and the number of bits-per-pixel.

The pixels array is stored after the MicroEJ image file header. A padding between the header and the pixels array is added to force to start the pixels array at a memory address aligned on number of bits-per-pixels.

xxx image RAW: format + padding + pixels

The available standard encodings are part of this list: ``ARGB8888 | RGB888 | RGB565 | ARGB1555 | ARGB4444 | A8 | A4 | A2 | A1 | C4 | C2 | C1 | AC44 | AC22 | AC11``

-  ARGB8888: 32 bits format, 8 bits for transparency, 8 per color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return c;
      }

-  RGB888: 24 bits format, 8 per color. Image is always fully opaque.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return c & 0xffffff;
      }

-  ARGB4444: 16 bits format, 4 bits for transparency, 4 per color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
                  | ((c & 0xf0000000) >> 16)
                  | ((c & 0x00f00000) >> 12)
                  | ((c & 0x0000f000) >> 8)
                  | ((c & 0x000000f0) >> 4)
                  ;
      }

-  ARGB1555: 16 bits format, 1 bit for transparency, 5 per color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
                  | (((c & 0xff000000) == 0xff000000) ? 0x8000 : 0)
                  | ((c & 0xf80000) >> 9)
                  | ((c & 0x00f800) >> 6)
                  | ((c & 0x0000f8) >> 3)
                  ;
      }

-  RGB565: 16 bits format, 5 or 6 per color. Image is always fully
   opaque.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
                  | ((c & 0xf80000) >> 8)
                  | ((c & 0x00fc00) >> 5)
                  | ((c & 0x0000f8) >> 3)
                  ;
      }

-  A8: 8 bits format, only transparency is encoded. The color to apply
   when drawing the image, is the current GraphicsContext color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0xff - (toGrayscale(c) & 0xff);
      }

-  A4: 4 bits format, only transparency is encoded. The color to apply
   when drawing the image, is the current GraphicsContext color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (0xff - (toGrayscale(c) & 0xff)) / 0x11;
      }

-  A2: 2 bits format, only transparency is encoded. The color to apply
   when drawing the image, is the current GraphicsContext color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (0xff - (toGrayscale(c) & 0xff)) / 0x55;
      }

-  A1: 1 bit format, only transparency is encoded. The color to apply
   when drawing the image, is the current GraphicsContext color.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (0xff - (toGrayscale(c) & 0xff)) / 0xff;
      }

-  C4: 4 bits format with grayscale conversion. Image is always fully
   opaque.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (toGrayscale(c) & 0xff) / 0x11;
      }

-  C2: 2 bits format with grayscale conversion. Image is always fully
   opaque.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (toGrayscale(c) & 0xff) / 0x55;
      }

-  C1: 1 bit format with grayscale conversion. Image is always fully
   opaque.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return (toGrayscale(c) & 0xff) / 0xff;
      }

-  AC44: 4 bits for transparency, 4 bits with grayscale conversion.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
              | ((color >> 24) & 0xf0)
              | ((toGrayscale(color) & 0xff) / 0x11)
              ;
      }

-  AC22: 2 bits for transparency, 2 bits with grayscale conversion.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
              | ((color >> 28) & 0xc0)
              | ((toGrayscale(color) & 0xff) / 0x55)
              ;
      }

-  AC11: 1 bit for transparency, 1 bit with grayscale conversion.

   ::

      u32 convertARGB8888toRAWFormat(u32 c){
          return 0
              | ((c & 0xff000000) == 0xff000000 ? 0x2 : 0x0)
              | ((toGrayscale(color) & 0xff) / 0xff)
              ;
      }

The pixels order in MicroEJ file follows this rule:

   ::

         pixel_offset = (pixel_Y * image_width + pixel_X) * bpp / 8;

.. _section_image_display_raw:

MicroEJ Format: Display
=======================

The display can hold a pixel encoding which is not standard (see :ref:`display_pixel_structure`). The MicroEJ format can be customized to encode the pixel in same encoding than display. The number of bits-per-pixels and the pixel bits organisation is asked during the MicroEJ format generation and when the ``drawImage`` algorithms are running. If the image to encode contains some transparent pixels, the output file will embed the transparency according to the display’s implementation capacity. When all pixels are fully opaque, no extra information will be stored in the output file in order to free up some memory space.

.. note:: From Image Engine point of view, the format stays a MicroEJ format, readable by the Image Core.

Advantages:

* Encoding is identical to display encoding.
* Drawing an image is often very fast (simple memory copy when the display pixel encoding does not hold the opacity level)
* Supports opacity encoding.

Disadvantages:

* No compression: the image size in bytes is proportional to the number of pixels.

.. _section_image_custom_raw:

MicroEJ Format: GPU
===================

The MicroEJ format may be customized to be platform's GPU compatible. It can be enriched by one or several restrictions on the pixels array: 

* Its start address has to be aligned on a higher value than the number of bits-per-pixels. 
* A padding has to be added after each line (row stride).
* The MicroEJ format can hold a platform dependant header, located between MicroEJ format header (start of file) and pixels array. The MicroEJ format is designed to let the platform encodes and decodes this additional header. For Image Engine software algorithms, this header is useless and never used. 

.. note:: From Image Engine point of view, the format stays a MicroEJ format, readable by the Image Engine Core.

Advantages:

* Encoding is recognized by the GPU
* Drawing an image is often very fast
* Supports opacity encoding

Disadvantages:

* No compression: the image size in bytes is proportional to the number of pixels.

xxx image raw: header + padding + customheader + pixels + line padding

.. note:: When a row stride is specified, the pixel offset rule becomes:

   ::

         pixel_offset = (pixel_Y * stride + pixel_X) * bpp / 8;


MicroEJ Format: RLE1
====================

The Image Engine can display embedded images that are encoded into a compressed format which encodes several consecutive pixels into one or more 16-bits words. This encoding manages a maximum alpha level of 2 (alpha level is always assumed to be 2, even if the image is not
transparent).

-  Several consecutive pixels have the same color (2 words).

   -  First 16-bit word specifies how many consecutive pixels have the
      same color.

   -  Second 16-bit word is the pixels' color.

-  Several consecutive pixels have their own color  (1 + n words).

   -  First 16-bit word specifies how many consecutive pixels have their
      own color.

   -  Next 16-bit word is the next pixel color.

-  Several consecutive pixels are transparent (1 word).

   -  16-bit word specifies how many consecutive pixels are transparent.

Advantages:

* Supports 0 & 2 alpha encoding.
* Good compression when several consecutive pixels respect one of the three previous rules.

Disadvantages:

* Drawing an image is slightly slower than when using Display format.

.. _section_image_binary_raw:

Binary Format
=============

This format is not compatible with the Image Core and by MicroUI. It is can be used by MicroUI addon libraries which provide their own images managements. 

Advantages:

* Encoding is known by platform.
* Compression is inherent to the format itself.

Disadvantages:

* This format cannot be used to target a MicroUI Image (unsupported format).
         
No Compression
==============

An image can be embedded without any conversion / compression. This allows to embed the resource as well, in order to keep the source image characteristics (compression, bpp etc.). This option produces the same result as specifying an image as a resource in the MicroEJ launcher.

Advantages:

* Conserves the image characteristics.

Disadvantages:

* Requires an image runtime decoder.
* Requires some RAM in which to store the decoded image in MicroEJ format.
