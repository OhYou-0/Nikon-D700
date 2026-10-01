# Nikon-D700

This is a modified firmware binary which removes the informational text and black boarders from the HDMI output.
It provides a clean HDMI video feed, so that the camera live feed can be captured and used as a video source as is without cropping. 

1. Redirect one HDMI scaling call — 4 bytes: Changes the function address loaded at 0x14B04C from the stock scaler wrapper (0x14543C) to the added wrapper (0x2AB600). The call instruction itself stays unchanged.

2. Add conditional cropping code — 52 bytes: Checks the inferred Live View/HD flags and output mode. For 720p or 1080i, it selects the crop descriptor and full-frame destination. Otherwise, it retains the original arguments. Both paths then call the stock scaler wrapper.

3. Add crop dimensions and buffer offsets — 28 bytes: Defines a 1504×846, exactly 16:9 crop centered vertically within the original 1504×1000 image. It adjusts the Y/U/V plane offsets while preserving their existing row strides, allowing proportional scaling without stretching.

4. Recalculate B firmware checksum — 2 bytes: Replaces B’s CRC-16/XMODEM with 0xE27D, calculated over the modified B component. A firmware and its checksum remain unchanged.

This was only tested from firmware A 1.04 B 1.03, if your current firmware is not that, I highly suggest that you update to that version first from official Nikon sources. In the unlikely event there is new firmware published for this camera, you should not update to it without restoring stock firmware of this version. 

There is no documented restore path or recovery, if the firmware update fails to boot, there is likely no restoration process and the camera is ruined. It worked in one single test, understand these risks before flashing. Understand that these cameras are very old at this point and your battery health should be considered a risk when flashing. The flash process takes 1-2 minutes and must not be interrupted. 
