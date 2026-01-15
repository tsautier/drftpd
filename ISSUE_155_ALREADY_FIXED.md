# Issue #155 - Slow Kick Fix

## Status: ALREADY FIXED

## Analysis
The issue reported problems with slow kick logic in Transfer.java:
1. irst variable not useful
2. lastCheck not updated
3. Announce shows _minSpeed instead of getXferSpeed()

## Current Code Status (lines 477-492)
The code has been corrected:
- lastCheck IS now updated (line 491): lastCheck = System.currentTimeMillis();
- Check intervals are proper: first check at 500ms, subsequent at 200ms
- Logging shows actual speed: getTransferSpeed() (line 485)

## Conclusion
This issue appears to have been fixed in a previous commit.
No changes needed.
