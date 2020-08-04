.. _section_image_core:

=================
Image Engine Core
=================


..


   image core
   - ne prend que des raw en entree (cf chapitre raw)
   - sait lire un pixel (convert en ARGB8888)
   - sait draw avec ou sans global alpha
   - rotation, flip, scale
   - llapi
   - format custom -> llapi


Principle
=========

The Image Engine Core module is an on-board engine that read and draw the image encoded in MicroEJ format. It calls low-level APIs to draw and transform the images (rotation, scaling, deformation etc.). It also includes software algorithms to perform the rendering.

raw + microui api -> core -> llapi
llapi softalgo -> core (soft algo)








Dependencies
============

-  MicroUI module (see :ref:`section_microui`)

-  Display module (see :ref:`section_display`)


Installation
============

Image Engine Core modules are part of the MicroUI module and Display
module. Install them in order to be able to use some images.


Use
===

The MicroUI image APIs are available in the class
``ej.microui.display.Image``.

..
   | Copyright 2008-2020, MicroEJ Corp. Content in this space is free 
   for read and redistribute. Except if otherwise stated, modification 
   is subject to MicroEJ Corp prior approval.
   | MicroEJ is a trademark of MicroEJ Corp. All other trademarks and 
   copyrights are the property of their respective owners.
