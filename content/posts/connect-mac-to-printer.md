+++
title = 'How to Connect Your Mac to Network Printer'
date = 2024-10-22T21:37:52+08:00
draft = false
tags = ["how to"]
+++

If you have a network printer shared by your Windows PC and want to connect your Mac to it (just like below), this post is perfect for you. You will need to make some configurations for your Windows PC and Mac separately.

```goat
.-----------.               .-----.
| Windows PC |<--Internet-->| Mac |
|  (Server)  |              '-----'
'-----+-----' <-.
      |          |
     usb      Internet
      |          |   .------------.
      v           '->| Windows PC |
 .---------.         '------------'
 | Printer |
 '---------'
```

## Windows (Server) Settings

First, your Windows PC has to have the right access to the printer, normally including installing the right driver and plugging a wire connecting your PC to the printer. Make sure that you can print using this Windows PC.

Before sharing your printer, please check if you have turned on some Windows features.

1. In the search box on the taskbar, type **Control Panel** and then select **Control Panel**.
2. Select **Programs** > **Turn Windows features on or off**.
3. Find **Print and Document Services** and turn on **Printer Server**, **LPD Print Service** (and **LPR Port Monitor** if you want to connect this computer to a network printer provided by another computer).

*Note: If you can use another PC to print via this network printer, but not a Mac, the reason is that the “Line Printer Daemon (LPD)” feature is not turned on at the PC as the server.*

Then, you can [share this printer](https://support.microsoft.com/en-us/windows/share-your-network-printer-c9a152b5-59f3-b6f3-c99f-f39e5bf664c3):

1. Select the **Start** button, then select **Settings** > **Devices** > **Printers & scanners**.
2. Choose the printer you want to share, then select **Manage**.
3. Select **Printer Properties**, then choose the **Sharing** tab.
4. On the Sharing tab, select **Share this printer**.
5. If you want, edit the Share name of the printer. You'll use this name to connect to the printer from your Mac or a secondary PC. When you are done, select **OK**.

Normally, you can go to MacOS settings now. But I suggest you finish the steps below to make sure your firewall lets through printer sharing.

1. Select the **Start** button, then select **Settings** > **Network & Internet** > **Wi-Fi**.
2. Under **Related settings**, select **Change advanced sharing options**.
3. In the **Advanced sharing settings** dialog box, expand the **Private** section. Next, under **Network discovery**, select **Turn on network discovery**.
4. Under **File and printer sharing**, select **Turn on file and printer sharing**.

## MacOS (Client) Settings

1. Install the proper driver of the printer for your Mac.
2. **Settings** > **Printers & Scanners** > **Add Printer, Scanner or Fax...**.
3. Right click  any one of these **Default/IP/Windows** buttons. Select **Customize Toolbar** will appear. Then drag **Advanced** into the Toolbar, which will be used later.
4. Select **Advanced** and select/fill as below:
   - **Type**: “LPD/LPR Host or Printer”
   - **URL**: `lpd://hostname/printername`. **This is important. You have to type in exactly the same hostname or computer name of the Windows PC providing network printer service and the printer name.** In my case, my URL is  `lpd://192.168.3.173/HP-LaserJet-Professional-M1136-MFP`.
   - Use: Select the proper driver
5. Click the **Add** button.
