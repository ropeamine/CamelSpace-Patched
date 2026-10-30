# CamelSpace-Patched

CamelSpace 1.501 AU - GUI fix for macOS 10.14+
lockFocus/unlockFocus NOP'd in VSTGUI_NSView_drawRect (64-bit + 32-bit).
  x86_64 slice 0x744bd, 0x745ab : ff15xxxxxxxx -> 660f1f440000
  i386   slice 0x84fda, 0x850f3 : e8xxxxxxxx   -> 0f1f440000
Nothing else changed (18 bytes total). License data untouched.
