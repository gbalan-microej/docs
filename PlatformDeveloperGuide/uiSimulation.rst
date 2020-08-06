.. _section_simulation:

==========
Simulation
==========


Principle
=========

A major strength of the MicroEJ environment is that it allows
applications to be developed and tested in a Simulator rather than on
the target device, which might not yet be built. To make this possible
for devices that have a display or controls operated by the user (such
as a touch screen or buttons), the Simulator must connect to a "mock" of
the control panel (the "Front Panel") of the device. The Front Panel generates a graphical representation of the
device, and is displayed in a window on the user's
development machine when the application is executed in the Simulator.
The Front Panel is the equivalent of the three embedded modules (Display,
Inputs and LED) of the MicroEJ Platform (see
:ref:`section_microui`).

The Front Panel enhances the development environment by allowing
User Interface  applications to be designed and tested on the computer
rather than on the target device (which may not yet be built). The mock
interacts with the user's computer in two ways:

-  output: LEDs, graphical displays

-  input: buttons, joystick, touch, haptic sensors

Functional Description
======================

1. Creates a new Front Panel project.

2. Creates an image of the required front panel. This could be a
   photograph or a drawing.

3. Defines the contents and layout of the front panel by editing an XML
   file (called an fp file). Full details about the structure and
   contents of fp files can be found in chapter
   :ref:`front_panel_file`.

4. Creates images to animate the operation of the controls (for example
   button down image).

5. Creates *Listeners* that generate the same MicroUI input events as
   the hardware.

6. Previews the front panel to check the layout of controls and the
   events they create, etc.

7. Exports the Front Panel project into a MicroEJ Platform project.


The Front Panel Project
=======================

Creating a Front Panel Project
------------------------------

A Front Panel project is created using the New Front Panel Project
wizard. Select:

``New > Project... > MicroEJ > Front Panel Project``

The wizard will appear:

.. figure:: images/newfp.png
   :alt: New Front Panel Project Wizard
   :align: center
   :width: 700px
   :height: 500px

   New Front Panel Project Wizard

Enter the name for the new project.

Project Contents
----------------

.. figure:: images/project-content.png
   :alt: Project Contents
   :align: center

   Project Contents

A Front Panel project has the following structure and contents:

* The ``src/main/java`` folder is provided for the definition of ``Listeners`` (and ``DisplayExtensions``). It is initially empty. The creation of these classes will be explained later.
* The ``src/main/resources`` folder holds the file or files that define the contents and layout of the front panel, with a ``.fp`` extension (the fp file or files), plus images used to create the front panel. A newly created project will have a single fp file with the same name as the project, as shown above. The contents of fp files are detailed later in this  document.
* The ``JRE System Library`` is referenced, because a Front Panel  project needs to support the writing of Java for the ``Listeners`` (and ``DisplayExtensions``).
* The ``Modules Dependencies`` contains the libraries for the front panel simulation, the widgets it supports and the types needed to implement ``Listeners`` (and ``DisplayExtensions``).
* The ``lib`` contains a local copy of ``Modules Dependencies``. 

Module Dependencies
===================

The Front Panel project is a standard IVY project. Its ``module.ivy`` file should look like this example:

::

   <ivy-module version="2.0" xmlns:ea="http://www.easyant.org" xmlns:ej="https://developer.microej.com" ej:version="2.0.0"> 
   <info organisation="com.mycompany" module="examplePanel" status="integration" revision="1.0.0"/>      

      <configurations defaultconfmapping="default->default;provided->provided">
         <conf name="default" visibility="public" description="Runtime dependencies to other artifacts"/>
         <conf name="provided" visibility="public" description="Compile-time dependencies to APIs provided by the platform"/>
      </configurations>

      <dependencies>
         <dependency org="ej.tool.frontpanel" name="widget" rev="1.0.0"/>
      </dependencies>
   </ivy-module>


It depends at least on the Front Panel framework. This framework contains the front panel core classes. The dependencies can be reduced to:

::

   <dependencies>
      <dependency org="ej.tool.frontpanel" name="framework" rev="1.1.0"/>
   </dependencies>

To be compatible with graphical engine, the project must depend on an extension of front panel framework. This extension provides some interfaces and classes the graphical engine is using to target simulated display and input devices. The extension does not provide any widgets. It is the equivalent of embedded low-level API. It fetches by transitivity the front panel framework, so no need to specify explicitely the front panel framework dependency: 

::

   <dependencies>
      <dependency org="com.microej.pack.ui" name="ui-pack" rev="13.0.0">
         <artifact name="frontpanel" type="jar"/>
      </dependency>
   </dependencies>

.. warning:: This extension is built for each UI pack version. By consequence a front panel project is made for a platform built with the same UI pack. When the UI packs mismatch, some errors may occur during the front panel project exporting step, during the platform build and/or during the application runtime

The graphical engine's front panel extension does not provide any widgets. Some compatible widgets are available in a third library. The cycle-life of this library is decorrelated of the UI pack cycle life. New widgets can be added to simulate new kind of displays, input devices etc. This extension fetches by transitivity the graphical engine's front panel extension , so no need to specify explicitely this extension dependency: 

::

   <dependencies>
      <dependency org="ej.tool.frontpanel" name="widgets" rev="2.0.0"/>
   </dependencies>

.. warning:: The minimal version ``2.0.0`` is required to be compatible with UI pack 13.0.0 and higher. By default, when creating a new front panel project, the widget dependency version is ``1.0.0``.


FP File
=======

File Contents
-------------

The Front Panel engine takes an XML file (the fp file) as input. It describes
the panel using widgets: They simulate the drivers, sensors and
actuators of the real device. The front panel engine generates the graphical
representation of the real device, and is displayed in a window on the
user's development machine when the application is executed in the
Simulator.

The following example file describes a typical board with one LCD, a
touch panel, three buttons, a joystick and four LEDs:

::

   <?xml version="1.0"?>
   <frontpanel 
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns="https://developer.microej.com" 
      xsi:schemaLocation="https://developer.microej.com .widget.xsd">
      
      <device name="MyBoard" skin="myboard.png">
         <ej.fp.widget.Display x="22" y="51" width="480" height="272"/>
         <ej.fp.widget.Pointer x="22" y="51" width="480" height="272" touch="true"/>
         
         <ej.fp.widget.LED label="0" x="30" y="352" ledOff="led0_0.png" ledOn="led0_1.png"/>		
         <ej.fp.widget.LED label="1" x="50" y="352" ledOff="led1_0.png" ledOn="led1_1.png"/>		
         <ej.fp.widget.LED label="2" x="70" y="352" ledOff="led2_0.png" ledOn="led2_1.png"/>		
         <ej.fp.widget.LED label="3" x="90" y="352" ledOff="led3_0.png" ledOn="led3_1.png"/>
         
         <ej.fp.widget.RepeatButton label="0" x="125" y="345" skin="button0.png" pushedSkin="button1.png"/>
         <ej.fp.widget.RepeatButton label="1" x="169" y="345" skin="button0.png" pushedSkin="button1.png"/>
         <ej.fp.widget.RepeatButton label="2" x="213" y="345" skin="button0.png" pushedSkin="button1.png"/>
         
         <ej.fp.widget.Joystick x="300" y="341" upSkin="joystick-UP.png" downSkin="joystick-DOWN.png" rightSkin="joystick-RIGHT.png" leftSkin="joystick-LEFT.png" skin="joystick-0.png"/>		
      </device>
   </frontpanel>

The ``device`` ``skin`` must refer to a ``png`` file in the
``src/main/resources`` folder. This image is used to render the background of the
front panel. The widgets are drawn on top of this background.

The ``device`` contains the elements that define the widgets that
make up the front panel. The name of the widget element defines the type
of widget. The set of valid types is determined by the Front Panel
Designer. Every widget element defines a ``label``, which must be unique
for widgets of this type (optional or not), and the ``x`` and ``y`` coordinates of the
position of the widget within the front panel (0,0 is top left). There
may be other attributes depending on the type of the widget.

The file and tags specifications are available in chapter
:ref:`front_panel_file`.

Working with fp Files
---------------------

To edit an fp file, open it using the Eclipse XML editor (right-click on
the fp file, select ``Open With > XML Editor``). This editor features
syntax highlighting and checking, and content-assist based on the schema
(XSD file) referenced in the fp file. This schema is a hidden file
within the project's definitions folder. An incremental builder checks
the contents of the fp file each time it is saved and highlights
problems in the Eclipse Problems view, and with markers on the fp file
itself.

A preview of the front panel can be obtained by opening the Front Panel
Preview
(``Window > Show View > Other... > MicroEJ > Front Panel Preview``).

The preview updates each time the fp file is saved.

A typical working layout is shown below.

.. figure:: images/working-layout.png
   :alt: Working Layout Example
   :align: center

   Working Layout Example

Within the XML editor, content-assist is obtained by pressing
ctrl+space.  The editor will list all the elements valid at the cursor
position, and insert a template for the selected element.

Input Device Filters
--------------------

The widgets which simulate the input devices use images (or "skins") to
show their current states (pressed and released). The user can change
the state of the widget by clicking anywhere on the skin: it is the
active area. This active area is, by default, rectangular.

These skins can be associated with an additional image called a
``filter``. This image defines the widget's active area. It
is useful when the widget is not rectangular.

.. figure:: images/fp-widget-active-area.*
   :alt: Active Area
   :width: 25.0%
   :align: center

   Active Area

The filter image must have the same size as the skin image. The active
area is delimited by the fully opaque pixels. Every pixel in the
filter image which is not fully opaque is considered not part of the
active area.

Display Filter
--------------

By default, a display area is rectangular. Some displays can have
another appearance (for instance: circular). The front panel is able to
simulate that using a filter. This filter defines the pixels inside and
outside the real display area. The filter image must have the same size
than display rectangular area. A display pixel at a given position will
be not rendered if the pixel at the same position in mask is fully
transparent.


Inputs Extensions
=================

The input device widgets (button, joystick, touch etc.) require a listener to know how to react on input events (press, release, move etc.). The aim of this listener is to generate an event compatible with MicroUI ``EventGenerator``. Thereby, a button press action can become a MicroUI ``Buttons`` press event or a ``Command`` event or anything else. 

A MicroUI ``EventGenerator`` is known by its name. This name is fixed during the MicroUI static initialization (see :ref:`section_static_init`). To generate an event to a specific event generator, the widget has to use the event generator name as identifier. 

A front panel widget can:

* Force the behavior of an input action: the associated MicroUI ``EventGenerator`` type is hardcoded (``Buttons``, ``Pointer`` etc.), the event is hardcoded (for instance: widget button press action may be hardcoded on event generator ``Buttons`` and on the event `pressed`). Only the event generator name (identifier) should be editable by the front panel extension project.
* Propose a default behavior of an input action: contrary to first point, the front panel extension project is able to change the default behavior. For instance a joystick can simulate a MicroUI ``Pointer``.
* Do nothing: the widget requires the front panel extension project to give a listener. This listener will receive all widgets action (press, release, etc.) and will have to react on it. The action should be converted on a MicroUI ``EventGenerator`` event or might be dropped.

This choice of behavior is widget dependant. Please refer to the widget documentation to have more information about the chosen behavior.

Dependencies
============

-  MicroUI module (see :ref:`section_microui`).

-  Display module (see :ref:`section_display`): This module gives
   the characteristics of the graphical display that are useful for
   configuring the Front Panel.


.. _fp_installation:

Installation
============

Front Panel is an additional module for MicroUI library. When the
MicroUI module is installed, install this module in order to be able to
simulate UI drawings on the Simulator.

In the platform configuration file, check :guilabel:`UI` > :guilabel:`Front Panel` to
install the Front Panel module. When checked, the properties file
``frontpanel`` > ``frontpanel.properties`` is required during platform creation to
configure the module. This configuration step is used to identify and
configure the front panel.

The properties file must / can contain the following properties:

-  ``project.name`` [mandatory]: Defines the name of the front panel
   project (same workspace as the platform configuration project). If
   the project name does not exist, a new project will be created.

-  ``fpFile.name`` [optional, default value is "" (*empty*)]: Defines
   the front panel file (\*.fp) to export (in case "project.name"
   contains several fp files). If empty or unspecified, the first ".fp"
   file found will be exported.


Use
===

Launch a MicroUI application on the Simulator to run the Front Panel.

..
   | Copyright 2008-2020, MicroEJ Corp. Content in this space is free 
   for read and redistribute. Except if otherwise stated, modification 
   is subject to MicroEJ Corp prior approval.
   | MicroEJ is a trademark of MicroEJ Corp. All other trademarks and 
   copyrights are the property of their respective owners.
