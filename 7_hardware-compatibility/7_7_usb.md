## 7.7\. USB

Device implementations SHOULD support USB peripheral mode and SHOULD support USB
host mode.

### 7.7.1\. USB peripheral mode

If a device implementation includes a USB port supporting peripheral mode:

*   The port MUST be connectable to a USB host that has a standard type-A or
    type-C USB port.
*   The port SHOULD use micro-B, micro-AB or Type-C USB form factor. Existing
    and new Android devices are **STRONGLY RECOMMENDED to meet these
    requirements** so they will be able to upgrade to the future platform
    releases.
*   The port SHOULD be located on the bottom of the device
    (according to natural orientation) or enable software screen rotation for
    all apps (including home screen), so that the display draws correctly when
    the device is oriented with the port at bottom. Existing and new Android
    devices are **STRONGLY RECOMMENDED to meet these requirements** so they will
    be able to upgrade to future platform releases.
*   It MUST allow a USB host connected with the Android device to access the
    contents of the shared storage volume using either USB mass storage or Media
    Transfer Protocol.
*   It SHOULD implement the Android Open Accessory (AOA) API and specification
    as documented in the Android SDK documentation, and if it is an Android
    Handheld device it MUST implement the AOA API. Device implementations
    implementing the AOA specification:
    *   MUST declare support for the hardware feature
        [android.hardware.usb.accessory](http://developer.android.com/guide/topics/connectivity/usb/accessory.html).
    *   SHOULD NOT implement [AOAv2 audio](https://source.android.com/devices/accessories/aoa2#audio-support)
        documented in the Android Open Accessory Protocol 2.0 documentation.
        AOAv2 audio is deprecated as of Android version 8.0 (API level 26).
    *   The USB mass storage class MUST include the string "android" at the end
        of the interface description `iInterface` string of the USB mass storage
*   It SHOULD implement support to draw 1.5 A current during HS chirp and
    traffic as specified in the [USB Battery Charging specification, revision 1.2](http://www.usb.org/developers/docs/devclass_docs/BCv1.2_070312.zip).
    Existing and new Android devices are **STRONGLY RECOMMENDED to meet these
    requirements** so they will be able to upgrade to the future platform
    releases.
*   Type-C devices MUST detect 1.5A and 3.0A chargers per the Type-C resistor
    standard and it must detect changes in the advertisement.
*   Type-C devices also supporting USB host mode are STRONGLY RECOMMENDED to
    support Power Delivery for data and power role swapping.
*   Type-C devices SHOULD support Power Delivery for high-voltage charging and
    support for Alternate Modes such as display out.
*   The value of iSerialNumber in USB standard device descriptor MUST be equal
    to the value of android.os.Build.SERIAL.
*   Type-C devices are STRONGLY RECOMMENDED to not support proprietary charging
    methods that modify Vbus voltage beyond default levels, or alter sink/source
    roles as such may result in interoperability issues with the chargers or
    devices that support the standard USB Power Delivery methods. While this is
    called out as "STRONGLY RECOMMENDED", in future Android versions we might
    REQUIRE all type-C devices to support full interoperability with standard
    type-C chargers.

### 7.7.2\. USB host mode

If a device implementation includes a USB port supporting host mode, it:

*   SHOULD use a type-C USB port, if the device implementation supports USB 3.1.
*   MAY use a non-standard port form factor, but if so MUST ship with a cable or
    cables adapting the port to a standard type-A or type-C USB port.
*   MAY use a micro-AB USB port, but if so SHOULD ship with a cable or cables adapting the port to a standard type-A or type-C USB port.
*   is **STRONGLY RECOMMENDED** to implement the
    [USB audio class](http://developer.android.com/reference/android/hardware/usb/UsbConstants.html#USB_CLASS_AUDIO)
    as documented in the Android SDK documentation. If the USB audio class is
    supported, it:
    *   MUST support the [USB HID
        class](https://developer.android.com/reference/android/hardware/usb/UsbConstants.html#USB_CLASS_HID)
    *   MUST support the detection and mapping of the following HID data fields
        specified in the [USB HID Usage
        Tables](http://www.usb.org/developers/hidpage/Hut1_12v2.pdf) and the
        [Voice Command Usage
        Request](http://www.usb.org/developers/hidpage/Voice_Command_Usage.pdf)
        to the [`KeyEvent`
        ](https://developer.android.com/reference/android/view/KeyEvent.html)
        constants as below:
        *   Usage Page (0xC) Usage ID (0x0CD): `KEYCODE_MEDIA_PLAY_PAUSE`
        *   Usage Page (0xC) Usage ID (0x0E9): `KEYCODE_VOLUME_UP`
        *   Usage Page (0xC) Usage ID (0x0EA): `KEYCODE_VOLUME_DOWN`
        *   Usage Page (0xC) Usage ID (0x0CF): `KEYCODE_VOICE_ASSIST`
*   MUST implement the Android USB host API as documented in the Android SDK,
    and MUST declare support for the hardware feature
    [android.hardware.usb.host](http://developer.android.com/guide/topics/connectivity/usb/host.html).
*   SHOULD support device charging while in host mode; advertising a source
    current of at least 1.5A as specified in the Termination Parameters section
    of the [USB Type-C Cable and Connector Specification Revision 1.2] (http://www.usb.org/developers/docs/usb_31_021517.zip)
    for USB Type-C connectors or using Charging Downstream Port(CDP) output
    current range as specified in the [USB Battery Charging specifications, revision 1.2](http://www.usb.org/developers/docs/devclass_docs/BCv1.2_070312.zip)
    for Micro-AB connectors.
*   USB Type-C devices are STRONGLY RECOMMENDED to support DisplayPort, SHOULD
    support USB SuperSpeed Data Rates, and are STRONGLY RECOMMENDED to support
    Power Delivery for data and power role swapping.
*   USB Type-C devices are STRONGLY RECOMMENDED to NOT support
    Audio Adapter Accessory Mode as described in the Appendix A of the
    [USB Type-C Cable and Connector Specification Revision 1.2](http://www.usb.org/developers/docs/).
*   Devices with any type-A or type-AB ports MUST NOT ship with an adapter converting
    from this port to a type-C receptacle.
*   MUST recognize any remotely connected MTP (Media Transfer Protocol) devices
    and make their contents accessible through the `ACTION_GET_CONTENT`,
    `ACTION_OPEN_DOCUMENT`, and `ACTION_CREATE_DOCUMENT` intents, if the Storage Access
    Framework (SAF) is supported.
*   MUST, if using a Type-C USB port and including support for peripheral mode,
    implement Dual Role Port functionality as defined by the USB Type-C
    specification (section 4.5.1.3.3).
*   SHOULD, if the Dual Role Port functionality is supported, implement the
    Try.\* model that is most appropriate for the device form factor. For
    example a handheld device SHOULD implement the Try.SNK model.
