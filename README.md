# VistaAlwaysGlass
###### Prevent Windows Vista from disabling Aero Glass transparency when windows are maximised! (… Yeah, I'm not creative with names.)

I recently decided to dig out and install Windows Vista Ultimate on a virtual machine just for the nostalgia and remembered the age-old annoyance where Aero Glass's transparency would inexplicably disable itself if a window was maximised. (To this day, I _still_ have no idea why Microsoft ever made that decision…)

Back in the day, this strange behaviour could just be patched out by way of a tool called "VistaGlazz" (by CodeGazer), but that software has since been lost to time as the developer's servers actually no longer host _any_ files related to it — so even if you manage to dig out a copy of the installer from the Internet Archive (trust me, I tried this), the installer actually _still_ fails due to it having to download files from the developer's server… which no longer hosts anything related to VistaGlazz.

As a result, I decided to figure out myself how to manually disable this annoying behaviour, and decided to share my findings/procedure here for others who may want to do the same.

**※ DISCLAIMER:** This is probably not the _exact_ same method that VistaGlazz used, but the end result is the same as far as I can tell, so… I think it's fine. ;P

## Installation instructions
This procedure has been tested to work on both 64-bit and 32-bit versions of Windows Vista Ultimate SP2, on both English and Japanese installations.

1. Download [deepxw's Universal Theme Patcher](http://deepxw.blogspot.com/2008/11/universal-theme-patcher.html) (from "Download link 3") and use it to patch all of the listed DLLs.
	* **※ IMPORTANT:** If you are using a 64-bit version of Windows Vista, you will need to run _both_ the x64 (64-bit) _and_ x86 (32-bit) versions of the patcher! (※ The first reboot prompt can be ignored — you only have to reboot once.)
2. Open a privileged `cmd` instance ("Run as Administrator") and take full ownership of `aero.msstyles` by running `takeown /f "%SYSTEMROOT%\Resources\Themes\Aero\aero.msstyles" && icacls "%SYSTEMROOT%\Resources\Themes\Aero\aero.msstyles" /grant administrators:F`.
3. Go to `%SYSTEMROOT%\Resources\Themes\Aero\` in an Explorer window. This will be referred to as the **System Aero Directory** in this README from now on.
4. Make a backup copy of `aero.msstyles`, naming it `aero.orig.msstyles`.
5. Disable Aero by switching to the Windows Vista Basic theme.
6. Download and run [xdelta UI](http://www.romhacking.net/utilities/598).
	* If you are familiar with the [xdelta CLI](https://github.com/jmacd/xdelta-gpl/releases), feel free to use that instead.
7. **For the patch**, choose the flavour of `VistaAlwaysGlass.xdelta` that corresponds to the architecture of your Windows installation (64-bit or 32-bit).
8. **For the source file**, choose `aero.orig.msstyles`.
9. **For the output file**, just set the filename to `aero.msstyles` and save it somewhere where you have write permissions, like your home folder or Desktop.
	* **※ NOTE:** It _is_ also possible to directly replace the copy in the **System Aero Directory** in this step if you run xdelta with administrative privileges. If you choose to do this, skip the next step.
10. Once completed, just replace the `aero.msstyles` file found in the **System Aero Directory** with your patched copy.
11. Re-enable Aero and maximised windows should now remain transparent! (No reboot required ;P)

## FAQ
* **Help! I'm getting a "checksum mismatch" error when I try to patch the file!**
	* Make sure you have Service Pack 2 installed. Earlier versions of Windows Vista have a slightly different `aero.msstyles` file.
	* Make sure you are using the correct patch corresponding to the architecture of your Windows installation (64-bit or 32-bit).
		* If you don't know which architecture you are using, open the Start Menu, right-click on Computer, and then click on Properties. The architecture of your Windows installation will be displayed under "System Type" in the window that opens.
	* If it _still_ fails even after verifying the above things, please manually patch your `aero.msstyles` file using the instructions below, and then [file an issue](https://github.com/akemin-dayo/VistaAlwaysGlass/issues/new)!
* **Why do we have to use xdelta to patch `aero.msstyles` instead of simply just replacing it with a pre-patched copy? This just makes the process more complicated…** ⊂⌒~⊃｡Д｡🍍)⊃
	* Simply redistributing the contents of `aero.msstyles` like that would be blatant copyright infringement.
	* Besides, given how this patch only changes one byte anyway… this also makes for a much smaller file size. ;P

## How to manually patch `aero.msstyles`
1. Download and install [NTCore Explorer Suite](https://ntcore.com/?page_id=388).
2. Run CFF Explorer and open `%SYSTEMROOT%\Resources\Themes\Aero\aero.msstyles`.
3. In the sidebar, go to "Resource Editor", then click on `VARIANT` → `NORMAL`.
4. Jump to offset `0x00023c43` (the blue arrow icon) and replace the `FF` with `00`.
5. Save your patched `aero.msstyles` to a separate location (using "File" → "Save As…").
6. Follow the same instructions as above, but _skip_ steps 6 and 7 and instead simply replace `aero.msstyles` with the one you just made.
