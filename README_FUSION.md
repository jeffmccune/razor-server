# VMware Fusion Setup

This document describes how to get a development environment running for Razor
using a Mac OS X workstation and VMware Fusion.

The Razor server will run on the Mac OS X system itself.  The VMware Fusion ISC
DHCP server will be used for DHCP.  The Mac OS X built in tftp server will also
be used.

Configure the TFTP server to run now and at boot:

    sudo launchctl load -w /System/Library/LaunchDaemons/tftp.plist

If you want to unload the TFTP server in the future, do so with:

    sudo launchctl unload -w /System/Library/LaunchDaemons/tftp.plist

Follow the PXE setup instructions in the README.md file.  Please note that the
TFTP root is `/private/tftpboot`, and the ISC DHCP configuration file is
`/Library/Preferences/VMware Fusion/vmnet8/dhcpd.conf`.

Sample DHCP Configuration:

    diff --git a/dhcpd.conf.orig b/dhcpd.conf
    index 40981c7..cf8493e 100644
    --- a/dhcpd.conf.orig
    +++ b/dhcpd.conf
    @@ -33,4 +33,8 @@ subnet 172.16.214.0 netmask 255.255.255.0 {
            option netbios-name-servers 172.16.214.2;
            option routers 172.16.214.2;
    +
    +  # Razor Development
    +  filename "undionly.kpxe";
    +  next-server 172.16.214.1;
     }
     host vmnet8 {

After modifying the DHCP configuration, VMware Fusion networking needs to be restarted using:

    sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --stop
    sudo "/Applications/VMware Fusion.app/Contents/Library/vmnet-cli" --start
