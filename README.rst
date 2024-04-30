============
Tardigrade Marine Debug Probe
============

Revision 0.1
============

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
