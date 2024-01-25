============
Tardigrade Marine Debug Probe
============

Revision 0.1
============

User guide
----------

1. Install pyOCD
2. Connect debug probe over USB
3. Confirm debug probe is visible run the following in the terminal: ``pyocd list``

Example output::
  
    #   Probe/Board                                            Unique ID   Target
  ---------------------------------------------------------------------------------
    0   Segger J-Link EDU Mini                                 801048415   n/a
  
    1   Tardigrade Marine Combined VCP and CMSIS-DAP Adapter   F3B00BF5    n/a

4. Look for the CMSIS package containing your target device ``pyocd pack find <partial_name_of_target_device>``, where ``<partial_name_of_target_device>`` can be ``samd21`` 
   to list all packages for Microchip SAMD21 devices. When run for the first time this might take a while.

Example output::

    Part            Vendor      Pack                   Version   Installed
  --------------------------------------------------------------------------
    ATSAMD21E15A    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E15B    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E15BU   Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E15CU   Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E15L    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E16A    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E16B    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E16BU   Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E16CU   Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E16L    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E17A    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E17D    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E17DU   Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E17L    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ATSAMD21E18A    Microchip   Microchip.SAMD21_DFP   3.6.144   False
    ...
    ATSAMD21J18A    Microchip   Microchip.SAMD21_DFP   3.6.144   False

5. Install the proper CMSIS package ``pyocd pack install <part_name>``, where ``<part_name>`` can for example be ``atsamd21e18a``, taken from the list at the previous step.

Example output::

  Downloading packs (press Control-C to cancel):
      Microchip.SAMD21_DFP.3.6.144
  Downloading descriptors (001/001)

6. Connect the debug probe to the target device
7. Erase complete target device ``pyocd erase --target <part_name> --chip``, where ``<part_name>`` can for example be ``atsamd21e18a``

Example output::

  0001803 I Erasing chip... [eraser]
  0001937 I Erasing sector 0x00000000 (16384 bytes) [eraser]
  0002001 I Erasing sector 0x00004000 (16384 bytes) [eraser]
  0002065 I Erasing sector 0x00008000 (16384 bytes) [eraser]
  0002130 I Erasing sector 0x0000c000 (16384 bytes) [eraser]
  0002196 I Erasing sector 0x00010000 (16384 bytes) [eraser]
  0002259 I Erasing sector 0x00014000 (16384 bytes) [eraser]
  0002323 I Erasing sector 0x00018000 (16384 bytes) [eraser]
  0002387 I Erasing sector 0x0001c000 (16384 bytes) [eraser]
  0002451 I Erasing sector 0x00020000 (16384 bytes) [eraser]
  0002516 I Erasing sector 0x00024000 (16384 bytes) [eraser]
  0002582 I Erasing sector 0x00028000 (16384 bytes) [eraser]
  0002652 I Erasing sector 0x0002c000 (16384 bytes) [eraser]
  0002729 I Erasing sector 0x00030000 (16384 bytes) [eraser]
  0002804 I Erasing sector 0x00034000 (16384 bytes) [eraser]
  0002868 I Erasing sector 0x00038000 (16384 bytes) [eraser]
  0002937 I Erasing sector 0x0003c000 (16384 bytes) [eraser]
  0003016 I Chip erase complete [eraser]

8. Program the target device using the command: ``pyocd flash --target <part_name> <path_to_binary>``, where ``<part_name>`` can for example be ``atsamd21e18a``
   and ``<path_to_binary>`` is the path to either the .bin or .hex file to be programmed

Example output::

  0001687 I Loading C:\Users\sebas\Downloads\Telegram Desktop\TM-dap_d21_nobl.bin [load_cmd]
  [==================================================] 100%
  0004486 I Erased 16384 bytes (1 sector), programmed 11264 bytes (176 pages), skipped 0 bytes (0 pages) at 3.94 kB/s [loader]

Hardware Design Description
---------------------------

Required Hand Modifications
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Replace U203 1.8V LDO with 3.3V LDO (LT1962EMS8-1.8 with LT1962EMS8-3.3)

Test Results
------------
  
✅ Voltage outputs OK: VUSB and 1V8

✅ SWD interface OK: connected and successfully erased using J-Link Segger and J-Flash Lite

✅ LEDs OK: VBUS; DBG_LED0; DBG_LED1; VCC; TGT_RESET (remark TGT_RESET led is on when reset is high so inactive ... might want to consider inverting LED in the future)

❌ USB interface not working at 1.8V: please see SAMD21 datasheet chapter 37.15: "The operating voltages must be 3.3V (Min. 3.0V, Max. 3.6V)."

✅ USB interface working at 3.3V

✅ Flashing using west and pyOCD working

Planned Design Changes
----------------------

- Replace LT1962-1.8 with a 3.3V regulator. Also look for a cheaper option since the LT part is around 5€
- Check BOM and investigate need for alternatives (optimise for availability and price)

Revision 0.2
============
