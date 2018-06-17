## Prepare

### Step 1 Get into RCM mode
  - Short circuit `pin1` and `pin10` at the right joy-con's comm port, then press Vol+ and Power. [How?](https://gbatemp.net/threads/paperclip-rcm-jig.502087/)
  - If screen still black, then you successfully entered RCM mode.
### Step 2 Prepare driver
  - Open `zadig-2.3.exe` with elevated privileges.
  - In `Options`, check `List All Devices`.
  - Select device `APX` in the device list, if you can't find, roll back to Step 1.
  - Select `libusbK (v3.0.7.0)` and click `Install Driver`.
### Step 3 Dump full NAND
  - Plugin an microSD card into your NX, 32GB+ is prefered.
  - Drag `hekate_ctcaer_2.3.bin` to `TegraRcmSmash.exe`.
  - Your NX should display something, if not, roll back to Step 1.
  - Move cursor with Vol buttons and press Power button to confirm.
  - Select `Tools` - `Dump eMMC boot` and wait for dump complete.
  - Plug microSD into your computer.
  - Move `BOOT0` to `NXHackTool` folder and rename it to `BOOT0.bin`, then empty your sdcard.
  - `Back` - `Reboot (RCM)`
  - Drag `hekate_ctcaer_2.3.bin` to `TegraRcmSmash.exe`.
  - Select `Tools` - `Dump raw eMMC` and wait for dump complete.
  - If your microSD is less than 32GB, you need to check [this](https://gbatemp.net/threads/rcm-payload-hekate-ctcaer-mod.502604/)
### Step 4 Get SBK, TSEK and BIS keys.
  - `Back` - `Reboot (RCM)`
  - Drag `biskeydump.bin` to `TegraRcmSmash.exe`.
  - Scan the QRCode displayed and save these keys to somewhere.
### Step 5 Prepare binarys
  - Open `HacDiskMount.exe` with elevated privileges.
  - `File` - `Open file` and select the rawnand.bin in your microSD, or if your sdcard is small, select the joined file in your disk.
  - Double click on `PRODINFO` and input the `BIS Key 0 Pair` you got in step 4.
  - In `Dump to file`, save it to `NXHackTool` folder with name `PRODINFO.bin`
  - Double click on `BCPKG2-1-Normal-Main` and input the `BIS Key 0 Pair` you got in step 4.
  - In `Dump to file`, save it to `NXHackTool` folder with name `BCPKG2-1-Normal-Main.bin`.
### Step 6 Derive Keys
  - Install `Python 2`.
  - Start `cmd` or `Powershell` in `NXHackTool` folder.
  - Run `python deriveKeys.py <Your SBK Key> <Your TSEC Key>` and wait for complete.
### Step 7 Get Game Files
  - Mount `USER` in `HacDistMount.exe`
  - Copy the desired `.nca` gamefile in `Content/registered`.
  - Copy the `8..0e2` file in `save` to `NXHackTool` folder.
  - Run `python getTicketbins.py 80000000000000e2` and wait.
  - Run `python getTitlekey.py PRODINFO.bin personal_ticketblob.bin` and leave it open.
  - Run `./hactool.exe -k keys.txt <Your .nca folder path>` and get the `Title ID`.
  - Find the titlekey which matches the `Title ID`.
  - Run `./hactool.exe -k keys.txt --titlekey=<Your titlekey> --romfsdir=romfs --exefsdir=exefs <Your .nca folder path>`.
  - The world now in peace.
