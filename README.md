# Nikon-D700

This is a modified firmware binary which removes the informational text and black boarders from the HDMI output.
It porivides a clean HDMI video feed, so that the camera live feed can be captured and used as a video source as is without cropping. 

Redirect one HDMI scaling call — 4 bytes: Changes the function address loaded at 0x14B04C from the stock scaler wrapper (0x14543C) to the added wrapper (0x2AB600). The call instruction itself stays unchanged.

Add conditional cropping code — 52 bytes: Checks the inferred Live View/HD flags and output mode. For 720p or 1080i, it selects the crop descriptor and full-frame destination. Otherwise, it retains the original arguments. Both paths then call the stock scaler wrapper.

Add crop dimensions and buffer offsets — 28 bytes: Defines a 1504×846, exactly 16:9 crop centered vertically within the original 1504×1000 image. It adjusts the Y/U/V plane offsets while preserving their existing row strides, allowing proportional scaling without stretching.

Recalculate B firmware checksum — 2 bytes: Replaces B’s CRC-16/XMODEM with 0xE27D, calculated over the modified B component. A firmware and its checksum remain unchanged.
