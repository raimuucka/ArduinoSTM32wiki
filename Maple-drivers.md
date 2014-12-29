Under Windows, Additional drivers need to be loaded to support the Maple / Maple mini upload process.
Unfortunately the drivers are not "signed" and by default Windows 7 and newer versions require signed drivers, and the maple drivers won't load.

The only reliable work-around under Widows 7, appears to be to disable the device driver signing enforcement during boot - by pressing F8

Other solutions may work for some people e.g. as documented here  http://www.killertechtips.com/2009/05/06/disable-driver-signing-in-windows-7-using-group-policy-editor/ .

Windows 8 / 8.1 appear to be able to permanently disable the device driver signing requirement. For example, as documented here https://learn.sparkfun.com/tutorials/disabling-driver-signature-on-windows-8/disabling-signed-driver-enforcement-on-windows-8