Licenses
========

Overview
--------

MicroEJ Architectures are distributed in two different versions:

- Evaluation Architectures, associated with a software license key. Can be downloaded at `<https://repository.microej.com/architectures/>`_.
- Production Architectures, associated with an hardware license key stored on a USB dongle. Can be requested to MicroEJ support team support@microej.com.

Licenses list is available in MicroEJ SDK preferences dialog page in :guilabel:`Window`
> :guilabel:`Preferences` > :guilabel:`MicroEJ`:

.. figure:: images/preferences/licenses.jpg
   :alt: MicroEJ Licenses View
   :align: center

   MicroEJ Licenses View

.. _license_manager:

License Manager
---------------

The license manager is provided with MicroEJ Architectures and this then integrated to Platforms, consequently:

- Evaluation licenses will be shown only if at least one Evaluation Architecture or Platform built from an Evaluation Architecture 
  has been imported in MicroEJ SDK.
- Production licenses will be shown only if at least one Production Architecture or Platform built from a Production Architecture 
  has been imported in MicroEJ SDK.

See sections :ref:`architecture_import` and :ref:`download.hardware.simulator` for more information.

.. _evaluation_license:

Evaluation Licenses
-------------------

This section should be considered when using Evaluation Architectures, which
use software license keys.

Get your Machine UID
~~~~~~~~~~~~~~~~~~~~

To activate an Evaluation Architecture, a machine UID needs to be provided
to the key server. 

This information is available from the preferences page:

- Go to :guilabel:`Window` > :guilabel:`Preferences` > :guilabel:`MicroEJ`,
- Select either :guilabel:`Architectures` or :guilabel:`Platforms`, 
- Click on one of the available Architectures or Platforms,
- Press the :guilabel:`Get UID` button to get the machine UID.

.. note:: 

   To access this :guilabel:`Get UID` option, at least one Evaluation Architecture must have been imported before (see :ref:`license_manager`).

Copy the UID. It will be needed when requesting a license.

.. figure:: images/preferences/uid.jpg
   :alt: Machine UID for Evaluation License
   :align: center
   :width: 532px
   :height: 172px

   Machine UID for Evaluation License

Request your Activation Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Go to https://license.microej.com.
- Click on :guilabel:`Create a new account` link.
- Create your account with a valid email address. You will receive a confirmation email a few minutes after. Click on the confirmation link in the email and login with your new account.
- Click on :guilabel:`Activate a License`.
- Set :guilabel:`Product P/N:` to ``9PEVNLDBU6IJ``.
- Set :guilabel:`UID:` to the UID you copied before.
- Click on :guilabel:`Activate`.
- The license is being activated. You should receive your activation by email in less than 5 minutes. If not, please contact support@microej.com.
- Once received by email, save the attached zip file that contains your activation key.

Install the License Key
~~~~~~~~~~~~~~~~~~~~~~~

- Go back to MicroEJ SDK.
- Select the :guilabel:`Window` > :guilabel:`Preferences` > :guilabel:`MicroEJ` menu.
- Press :guilabel:`Add...`.
- Browse the previously downloaded activation key archive file.
- Press OK. A new license is successfully installed.
- Go to Architectures sub-menu and check that all Architectures are now activated (green check).
- Your MicroEJ SDK is successfully activated.

If an error message appears, the license key could not be installed. (see
section :ref:`evaluation_license_troubleshooting`).
A license key can be removed from key-store by selecting it and by
clicking on :guilabel:`Remove` button.

.. _evaluation_license_troubleshooting:

Troubleshooting
~~~~~~~~~~~~~~~

Consider this section when an error message appears while adding the
Evaluation license key. Before contacting MicroEJ support, please check the
following conditions:

-  Key is corrupted (wrong copy/paste, missing characters or extra
   characters)

-  Key has not been generated for the installed environment

-  Key has not been generated with the machine UID

-  Machine UID has changed since submitting license request and no
   longer matches license key

-  Key has not been generated for one of the installed Architectures (no
   license manager able to load this license)

.. figure:: images/preferences/wrongkey.jpg
   :alt: Invalid License Key Error Message
   :align: center
   :width: 532px
   :height: 210px

   Invalid License Key Error Message


.. _production_license:

Production Licenses
-------------------

This section should be considered when using Production Architectures,
which use hardware license keys stored on an USB dongle.

.. figure:: images/dongle/dongle.jpg
   :alt: MicroEJ USB Dongle
   :align: center
   :scale: 30%

   MicroEJ USB Dongle

.. note :: 

   If your USB dongle has been provided to you by your sales representative and you don't have received an activation certificate by email, it may be a pre-activated dongle.
   Then you can skip the activation steps and directly jump to :ref:`production_license_check` section.

Request your Activation Key
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Go to `license.microej.com <https://license.microej.com/>`_.
- Click on :guilabel:`Create a new account` link.
- Create your account with a valid email address. You will receive a confirmation email a few minutes after. Click on the confirmation link in the email and login with your new account.
- Click on :guilabel:`Activate a License`.
- Set :guilabel:`Product P/N:` to **The P/N on the activation certificate**.
- Enter your UID: serial number printed on the USB dongle label (8 alphanumeric char.).
- Click on :guilabel:`Activate` and check confirmation message.
- Click on :guilabel:`Confirm your registration`.
- Enter the **Registration Code provided on the activation certificate**.
- Click on :guilabel:`Submit`.
- Your Activation Key will be sent to you by email as soon as it is available (12 business hours max.).

.. note:: 
   
   You can check the :guilabel:`My Products` page to verify your product registration status, the Activation Key availability and to download the Activation Key when available.

Once the Activation Key is available, download and save the Activation Key ZIP file to a local directory.

Activate your USB Dongle
~~~~~~~~~~~~~~~~~~~~~~~~

This section contains instructions that will allow to flash your
USB dongle with the proper activation key.

You shall ensure that the following prerequisites are met :

-  The USB dongle is plugged and recognized by your operating system
   (see :ref:`production_license_troubleshooting` section)

-  No more than one USB dongle is plugged to the computer while running the
   update tool

-  The update tool is not launched from a Network drive or from a USB
   key

-  The activation key you downloaded is the one for the dongle UID on
   the sticker attached to the dongle (each activation key is tied to
   the unique hardware ID of the dongle).

You can then proceed to the USB dongle update: 

- Unzip the ``Activation Key`` file to a local directory 
- Enter the directory just created by your ZIP extraction tool.
- Launch the executable program.
- Click on the :guilabel:`Update` button (no password needed)

  .. figure:: images/dongle/updateTool.png
     :alt: Dongle Update Tool
     :align: center
     :width: 271px
     :height: 310px

     Dongle Update Tool

- On success, an ``Update successfully`` message shall appear. On failure, an
  ``Error key or no proper rockey`` message may appear.

  .. figure:: images/dongle/updateSuccessful.png
     :alt: Successful dongle update
     :align: center
     :width: 222px
     :height: 169px

     Successful dongle update

.. _production_license_check:

Check Activation on MicroEJ SDK
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

   Production licenses will be shown only if at least one Production Architecture has been imported before (see :ref:`license_manager`).

- Go back to MicroEJ SDK,
- Go to :guilabel:`Window` > :guilabel:`Preferences` > :guilabel:`MicroEJ`,
- Go to :guilabel:`Architectures` or :guilabel:`Platforms` sub-menu and check that all Production Architectures or Platforms are now activated (green check).

.. figure:: images/dongle/platformLicenseDetails.png
   :alt: Platform License Status OK
   :align: center
   :width: 926px
   :height: 324px

   Platform License Status OK

.. _production_license_troubleshooting:

Troubleshooting
~~~~~~~~~~~~~~~

This section contains instructions to check that your
USB dongle is correctly recognized by your operating system.

GNU/Linux Troubleshooting
"""""""""""""""""""""""""

For GNU/Linux Users (Ubuntu at least), by default, the dongle access has
not been granted to the user, you have to modify udev rules. Please
create a ``/etc/udev/rules.d/91-usbdongle.rules`` file with the
following contents:

::

   ACTION!="add", GOTO="usbdongle_end"
       SUBSYSTEM=="usb", GOTO="usbdongle_start"
       SUBSYSTEMS=="usb", GOTO="usbdongle_start"
       GOTO="usbdongle_end"
       
       LABEL="usbdongle_start"
       
       ATTRS{idVendor}=="096e" , ATTRS{idProduct}=="0006" , MODE="0666"
       
       LABEL="usbdongle_end"

Then, restart udev: ``/etc/init.d/udev restart``

You can check that the device is recognized by running the ``lsusb``
command. The output of the command should contain a line similar to the
one below for each dongle :
``Bus 002 Device 003: ID 096e:0006 Feitian Technologies, Inc.``

Windows Troubleshooting
"""""""""""""""""""""""

For Windows users, each dongle shall be recognized with the following
hardware ID :

::

   HID\VID_096E&PID_0006&REV_0109

On Windows 8.1, go to :guilabel:`Device Manager` > :guilabel:`Human Interface Devices` and
check among the ``USB Input Device`` entries that the
``Details`` > ``Hardware Ids`` property match the ID mentioned before.

..
   | Copyright 2008-2020, MicroEJ Corp. Content in this space is free 
   for read and redistribute. Except if otherwise stated, modification 
   is subject to MicroEJ Corp prior approval.
   | MicroEJ is a trademark of MicroEJ Corp. All other trademarks and 
   copyrights are the property of their respective owners.

VirtualBox Troubleshooting
""""""""""""""""""""""""""

In a VirtualBox virtual machine, USB drives must be enabled to be recognized correctly.
So make sure to enable the USB dongle by clicking on it in the VirtualBox menu ``Devices`` > ``USB``.

In order to make this setting persistent, go to ``Devices`` > ``USB`` > ``USB Settings...`` 
and add the USB dongle in the ``USB Devices Filters`` list.


..
   | Copyright 2008-2020, MicroEJ Corp. Content in this space is free 
   for read and redistribute. Except if otherwise stated, modification 
   is subject to MicroEJ Corp prior approval.
   | MicroEJ is a trademark of MicroEJ Corp. All other trademarks and 
   copyrights are the property of their respective owners.
