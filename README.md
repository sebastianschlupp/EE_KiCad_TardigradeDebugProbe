# Hardware Design Description

# Test Results
- Voltage outputs OK: VUSB and 1V8
- SWD interface OK: connected and successfully erased using J-Link Segger and J-Flash Lite
- LEDs OK: VBUS; DBG_LED0; DBG_LED1; VCC; TGT_RESET (remark TGT_RESET led is on when reset is high so inactive ... might want to consider inverting LED in the future)
