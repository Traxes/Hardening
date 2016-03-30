![](media/image1.emf){width="4.248031496062992in"
height="1.6181102362204725in"}

Table of Content {#table-of-content .ListParagraph .berschriftInhalt-ERNW}
================

[Introduction](#introduction)

[Authentication](#authentication)

[Disable Auto-login](#disable-auto-login)

[Enable Single User Mode Authentication](#enable-single-user-mode-authentication)

[Require Username and Password for Login](#require-username-and-password-for-login)

[Disable Password Hints](#disable-password-hints)

[Set Screensaver Inactivity Interval](#set-screensaver-inactivity-interval)

[Require Password to Unlock Screensaver](#require-password-to-unlock-screensaver)

[Restrict sudo Configuration](#restrict-sudo-configuration)

[Disable Unauthorized Administrative Access for Sessions Locked
Through Screensaver](#disable-unauthorized-administrative-access-for-sessions-locked-through-screensaver)

[System Security](#system-security)

[Automativally Lock Login Keychain](#automativally-lock-login-keychain)

[Change Initial Password for Login Keychain](#change-initial-password-for-login-keychain)

[Enable Automatic Updates](#enable-automatic-updates)

[Disable Guest Access](#disable-guest-access)

[Enable Gatekeeper](#enable-gatekeeper)

[Set EFI Password](#set-efi-password)

[Disable Core Dumps](#disable-core-dumps)

[Prevent Safari from Opening Known File Types](#prevent-safari-from-opening-known-file-types)

[3.9 Set Strict Global umask 8](#set-strict-global-umask)

[3.10 Set Strict Home Directory Permissions](#set-strict-home-directory-permissions)

[Enable Secure Erase of Deleted Files in Trash](#_Toc363064718)

[Implement Hard Disk Encryption](#implement-hard-disk-encryption)

[Network Security](#network-security)

[Disable Apple File Protocol (AFP)](#disable-apple-file-protocol-afp)

[Disable File Transfer Protocol (FTP) daemon](#disable-file-transfer-protocol-ftp-daemon)

[Disable File Sharing](#disable-file-sharing)

[Disable Printer Sharing](#disable-printer-sharing)

[Disable Additional and Unnecessary Services](#disable-additional-and-unnecessary-services)

[Set Hardened TCP/IP Kernel Parameters](#set-hardened-tcpip-kernel-parameters)

[Enable Network Time Synchronization via NTP](#enable-network-time-synchronization-via-ntp)

[Disable Bluetooth](#disable-bluetooth)

[Disable Location Services](#disable-location-services)

[Enable Firewall](#enable-firewall)

[Disable Wake-on-LAN](#disable-wake-on-lan)

[Limit IPv6 to Local Subnet/Disable IPv6](#limit-ipv6-to-local-subnetdisable-ipv6)

[Logging & Monitoring](#logging-monitoring)

[Enable BSM Audit](#enable-bsm-audit)

[Apendix: List of Services](#apendix-list-of-services)

Introduction
============

As no official hardening guide for Apple’s OS X Mountain Lion is
available yet, ERNW has compiled the most relevant settings into this
checklist. While there is a significant amount of controls that can be
applied, this document is supposed to provide a solid base of hardening
measures. Settings which might have severe impact on the functionality
of the operating system and need a lot of further testing are not part
of this checklist.

We have marked each recommended setting in this checklist either with
“mandatory” or “optional” to make a clear statement, which setting is a
MUST (mandatory) or a SHOULD (optional) from our point of view.
“Optional” also means that we recommend to apply this setting, but there
may be required functionality on the system that will become unavailable
once the setting is applied.

1.  Authentication
    ==============

2.  Disable Auto-login
    ------------------

  ----------------------------------------------------------------------------------------
  -   Go to *Security and Privacy* settings in the *System Preferences* menu   Mandatory
                                                                               
  -   Check *Disable automatic login*                                          
                                                                               
                                                                               
  ---------------------------------------------------------------------------- -----------
  ----------------------------------------------------------------------------------------

Enable Single User Mode Authentication 
---------------------------------------

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}Change *secure* to *insecure* in /etc/ttys   <span id="OLE_LINK1" class="anchor"><span id="OLE_LINK2" class="anchor"></span></span>Optional
                                                                                                                                      
  > If the root account is disabled, booting into single user mode is not possible.                                                   
  ----------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Require Username *and* Password for Login
-----------------------------------------

  -----------------------------------------------------------------------------------
  -   Go to *Users & Groups* settings in the *System Preferences* menu.   Mandatory
                                                                          
  -   At *Display login window as* select *Name and password*.            
                                                                          
                                                                          
  ----------------------------------------------------------------------- -----------
  -----------------------------------------------------------------------------------

Disable Password Hints
----------------------

  -----------------------------------------------------------------------------------
  -   Go to *Users & Groups* settings in the *System Preferences* menu.   Mandatory
                                                                          
  -   Choose *Login options.*                                             
                                                                          
  -   Uncheck *Show password hints.*                                      
                                                                          
                                                                          
  ----------------------------------------------------------------------- -----------
  -----------------------------------------------------------------------------------

Set Screensaver Inactivity Interval 
------------------------------------

  -----------------------------------------------------------------------------------------------------------------------------------------
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Set the inactivity interval to 5min.   Mandatory
                                                                                                                                
  > defaults -currentHost write com.apple.screensaver idleTime -int 300                                                         
  ----------------------------------------------------------------------------------------------------------------------------- -----------
  -----------------------------------------------------------------------------------------------------------------------------------------

Require Password to Unlock Screensaver
--------------------------------------

  ---------------------------------------------------------------------------------------
  -   Go to *Security & Privacy* settings in the *System Preferences* menu.   Mandatory
                                                                              
  -   Choose tab *General.*                                                   
                                                                              
  -   Check *Require password \[…\] after sleep or screen saver begins.*      
                                                                              
  -   Set duration to *immediately.*                                          
                                                                              
                                                                              
  --------------------------------------------------------------------------- -----------
  ---------------------------------------------------------------------------------------

Restrict sudo Configuration
---------------------------

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Open the sudo configuration file:                                                   Mandatory
                                                                                                                                                                             
  sudo visudo                                                                                                                                                                
                                                                                                                                                                             
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Restrict sudo usage to one single command and to the authenticated terminal only:   
                                                                                                                                                                             
  Defaults timestamp\_timeout=0                                                                                                                                              
                                                                                                                                                                             
  Defaults tty\_tickets[^1]                                                                                                                                                  
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Disable Unauthorized Administrative Access for Sessions Locked Through Screensaver
----------------------------------------------------------------------------------

  ---------------------------------------------------------------------------------------------
  -   In /etc/authorization edit the section system.login.screensaver as follows:   Mandatory
                                                                                    
  > &lt;key&gt;system.login.screensaver&lt;/key&gt;                                 
  >                                                                                 
  > &lt;dict&gt;                                                                    
  >                                                                                 
  > &lt;key&gt;class&lt;/key&gt;                                                    
  >                                                                                 
  > &lt;string&gt;rule&lt;/string&gt;                                               
  >                                                                                 
  > &lt;key&gt;comment&lt;/key&gt;                                                  
  >                                                                                 
  > &lt;string&gt;The owner can unlock the screensaver.&lt;/string&gt;              
  >                                                                                 
  > &lt;key&gt;rule&lt;/key&gt;                                                     
  >                                                                                 
  > &lt;string&gt;*authenticate-session-owner-or-group*&lt;/string&gt;              
                                                                                    
  Go to the rules section and add the following element:                            
                                                                                    
  &lt;key&gt;authenticate-session-owner-or-group&lt;/key&gt;                        
                                                                                    
  &lt;dict&gt;                                                                      
                                                                                    
  &lt;key&gt;allow-root&lt;/key&gt;                                                 
                                                                                    
  &lt;false/&gt;                                                                    
                                                                                    
  &lt;key&gt;class&lt;/key&gt;                                                      
                                                                                    
  &lt;string&gt;user&lt;/string&gt;                                                 
                                                                                    
  &lt;key&gt;comment&lt;/key&gt;                                                    
                                                                                    
  &lt;string&gt;*your comment*&lt;/string&gt;                                       
                                                                                    
  &lt;key&gt;group&lt;/key&gt;                                                      
                                                                                    
  &lt;string&gt;*MAC-ADMIN-GROUP*&lt;/string&gt;                                    
                                                                                    
  &lt;key&gt;session-owner&lt;/key&gt;                                              
                                                                                    
  &lt;true/&gt;                                                                     
                                                                                    
  &lt;key&gt;shared&lt;/key&gt;                                                     
                                                                                    
  &lt;false/&gt;                                                                    
                                                                                    
  &lt;/dict&gt;                                                                     
  --------------------------------------------------------------------------------- -----------
  ---------------------------------------------------------------------------------------------

1.  System Security
    ===============

2.  Automativally Lock Login Keychain
    ---------------------------------

  --------------------------------------------------------------------------
  -   Open *Keychain Acces* and select the *login* keychain.     Mandatory
                                                                 
  -   Choose *Edit* → *Change Settings for KeychainI “login”.*   
                                                                 
  -   Set *Lock after \[…\] minutes of inactivity* to 10.        
                                                                 
  -   Check *Lock when sleeping.*                                
                                                                 
                                                                 
  -------------------------------------------------------------- -----------
  --------------------------------------------------------------------------

Change Initial Password for Login Keychain
------------------------------------------

  -------------------------------------------------------------------------
  -   Open *Keychain Acces* and select the *login* keychain.    Mandatory
                                                                
  -   Choose *Edit* → *Change Password for Keychain “login”.*   
                                                                
  -   Set a new password *different* to the login password.     
                                                                
                                                                
  ------------------------------------------------------------- -----------
  -------------------------------------------------------------------------

Enable Automatic Updates
------------------------

  ------------------------------------------------------------------------------
  -   Go to *App Store* settings in the *System Preferences* menu.   Mandatory
                                                                     
  -   Check *Automatically check for updates .*                      
                                                                     
  -   Check *Download newly available updates in the background.*    
                                                                     
  -   Check *Install app updates.*                                   
                                                                     
  -   Check *Install system data files and security updates.*[^2]    
                                                                     
                                                                     
  ------------------------------------------------------------------ -----------
  ------------------------------------------------------------------------------

Disable Guest Access
--------------------

  -----------------------------------------------------------------------------------
  -   Go to *Users & Groups* settings in the *System Preferences* menu.   Mandatory
                                                                          
  -   Choose the *Guest User.*                                            
                                                                          
  -   Uncheck *Allow guests to login into this computer.*                 
                                                                          
                                                                          
  ----------------------------------------------------------------------- -----------
  -----------------------------------------------------------------------------------

Enable Gatekeeper
-----------------

  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Security & Privacy.*                                                                                                                                                                                                                                                                                                                                Optional
                                                                                                                                                                                                                                                                                                                                                                                        
  -   Choose tab *General.*                                                                                                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                                                                                        
  -   ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}Set *Allow applications downloaded from* to *Mac App Store and identified Developers.*                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                                                                                                        
  This will prevent unsigned application bundles from being executed. This does not cover applications/binaries that are not bundles. Unsigned application bundles from trusted sources can be executed by performing a right-click on the application bundle, choose *Open*, and confirm the warning dialog with Open. An exception for this bundle will be generated automatically.   
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}Set EFI Password
---------------------------------------------------------------------------------------------------

  ----------------------------------------------------------------------------------------------------
  > Prevent unauthorized access to the EFI of the system by setting a firmware password.   Mandatory
                                                                                           
  -   Use the Firmware Password Utility to set a firmware password.                        
                                                                                           
  > This will require the password to be entered when booting into Single User, Verbose    
  >                                                                                        
  > or Target Disk mode as well as booting into the recovery mode (command-r).             
  ---------------------------------------------------------------------------------------- -----------
  ----------------------------------------------------------------------------------------------------

Disable Core Dumps
------------------

  ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"} launchctl limit core 0   Optional
  ------------------------------------------------------------------------------------------------------------ ----------

Prevent Safari from Opening Known File Types
--------------------------------------------

  --------------------------------------------------------------
  -   Launch the *Safari* browser application.       Mandatory
                                                     
  -   Choose *Preferences.*                          
                                                     
  -   Choose tab *General.*                          
                                                     
  -   Uncheck *Open safe files after downloading.*   
                                                     
                                                     
  -------------------------------------------------- -----------
  --------------------------------------------------------------

Set Strict Global umask
-----------------------

  -----------------------------------------------------------------------------------------------------------
  ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}              Optional
                                                                                                   
  sudo echo "umask 027" &gt;&gt; /etc/launchd.conf                                                 
                                                                                                   
  > ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}            
  >                                                                                                
  > This might break the installation of additional software that relies on a less strict umask.   
  ------------------------------------------------------------------------------------------------ ----------
  -----------------------------------------------------------------------------------------------------------

Set Strict Home Directory Permissions
-------------------------------------

  ------------------------------------------------------------------------------------------------
  ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}   Optional
                                                                                        
  sudo chmod 700 /Users/&lt;*username&gt;*                                              
  ------------------------------------------------------------------------------------- ----------
  ------------------------------------------------------------------------------------------------

<span id="_Toc363064718" class="anchor"><span id="OLE_LINK3" class="anchor"><span id="OLE_LINK4" class="anchor"></span></span></span>Enable Secure Erase of Deleted Files in Trash
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  --------------------------------------------------
  -   Launch the *Finder* application.   Mandatory
                                         
  -   Choose *Preferences.*              
                                         
  -   Click *Advanced….*                 
                                         
  -   Check *Empty Trash securely.*      
                                         
                                         
  -------------------------------------- -----------
  --------------------------------------------------

Implement Hard Disk Encryption
------------------------------

  -------------------------------------------------------------
  -   Launch the *System preferences* application.   Optional
                                                     
  -   Choose *Security & Privacy.*                   
                                                     
  -   Click *FileVault….*                            
                                                     
  -   Turn FileVault on.                             
                                                     
                                                     
  -------------------------------------------------- ----------
  -------------------------------------------------------------

1.  Network Security
    ================

2.  Disable Apple File Protocol (AFP)
    ---------------------------------

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Sharing.*                                                                                                                        Optional
                                                                                                                                                                     
  -   Select *File Sharing.*                                                                                                                                         
                                                                                                                                                                     
  -   Click *Options.*                                                                                                                                               
                                                                                                                                                                     
  -   Uncheck *Share files and folders using AFP.*                                                                                                                   
                                                                                                                                                                     
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Alternatively AFP can be disabled using the command line interface:         
                                                                                                                                                                     
  ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"} sudo launchctl unload -w /System/Library/LaunchDaemons/AppleFileServer.plist   
                                                                                                                                                                     
  Disabled per default on OS X 10.8.                                                                                                                                 
  ------------------------------------------------------------------------------------------------------------------------------------------------------------------ ----------
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Disable File Transfer Protocol (FTP) daemon
-------------------------------------------

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------
  ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}                                                                    Optional
                                                                                                                                                         
  ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"} sudo launchctl unload -w /System/Library/LaunchDaemons/ftp.plist   
                                                                                                                                                         
  Disabled per default on OS X 10.8.                                                                                                                     
  ------------------------------------------------------------------------------------------------------------------------------------------------------ ----------
  -----------------------------------------------------------------------------------------------------------------------------------------------------------------

Disable File Sharing
--------------------

  --------------------------------------------------------
  -   Go to *System Preferences* → *Sharing.*   Optional
                                                
  -   Uncheck *File Sharing.*                   
                                                
                                                
  --------------------------------------------- ----------
  --------------------------------------------------------

Disable Printer Sharing
-----------------------

  ------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Sharing.*                                                                         Optional
                                                                                                                      
  -   ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}Uncheck *Printer Sharing.*   
                                                                                                                      
  > Disabled per default on OS X 10.8.                                                                                
  ------------------------------------------------------------------------------------------------------------------- ----------
  ------------------------------------------------------------------------------------------------------------------------------

Disable Additional and Unnecessary Services
-------------------------------------------

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Disable services which are not needed or required by other applications/services.   Mandatory
                                                                                                                                                                             
  sudo launchctl unload -w *&lt;FullPathToPlistFile&gt;*                                                                                                                     
                                                                                                                                                                             
  -   Servicefiles (Plistfiles) are located in                                                                                                                               
                                                                                                                                                                             
      -   /System/Library/LaunchDaemons                                                                                                                                      
                                                                                                                                                                             
      -   /System/Library/LaunchAgents                                                                                                                                       
                                                                                                                                                                             
      -   /Library/LaunchDaemons                                                                                                                                             
                                                                                                                                                                             
      -   /Library/LaunchAgents                                                                                                                                              
                                                                                                                                                                             
      -   /Users/*USERNAME*/Library/LaunchDaemons                                                                                                                            
                                                                                                                                                                             
      -   ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"}/Users/*USERNAME*/Library/LaunchAgents                                          
                                                                                                                                                                             
  Before disabling a service it must be ensured that its functionality is not required by other                                                                              
                                                                                                                                                                             
  software components or services.                                                                                                                                           
  -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Set Hardened TCP/IP Kernel Parameters
-------------------------------------

  ------------------------------------------------------------------------------
  -   Set kernel parameters in /etc/sysctl.conf:                     Mandatory
                                                                     
      -   net.inet.ip.fw.verbose = 1                                 
                                                                     
      -   net.inet.ip.fw.verbose\_limit = 65535                      
                                                                     
      -   net.inet.icmp.icmplim = 1024                               
                                                                     
      -   net.inet.icmp.drop\_redirect = 1                           
                                                                     
      -   net.inet.icmp.log\_redirect = 1                            
                                                                     
      -   net.inet.ip.redirect = 0                                   
                                                                     
      -   net.inet.ip.sourceroute = 0                                
                                                                     
      -   net.inet.ip.accept\_sourceroute = 0                        
                                                                     
      -   net.inet.icmp.bmcastecho = 0                               
                                                                     
      -   net.inet.icmp.maskrepl = 0                                 
                                                                     
      -   net.inet.tcp.delayed\_ack = 0                              
                                                                     
      -   net.inet.ip.forwarding = 0                                 
                                                                     
      -   net.inet.tcp.strict\_rfc1948 = 1                           
                                                                     
  The system must be restarted before these changes become active.   
  ------------------------------------------------------------------ -----------
  ------------------------------------------------------------------------------

Enable Network Time Synchronization via NTP
-------------------------------------------

  ----------------------------------------------------------------------------------------------------------------------------
  -   Edit /private/etc/hostconfig and change TIMESYNC to YES.                                                     Mandatory
                                                                                                                   
  -   Configure the desired NTP server in /private/etc/ntp.conf through a corresponding server entry.              
                                                                                                                   
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Restart the NTP daemon.   
                                                                                                                   
  sudo launchctl load -w /System/Library/LaunchDaemons/org.ntp.ntpd.plist                                          
  ---------------------------------------------------------------------------------------------------------------- -----------
  ----------------------------------------------------------------------------------------------------------------------------

Disable Bluetooth
-----------------

  -------------------------------------------------------------------------
  -   Disbale Bluetooth in *System Preferences* → *Bluetooth.*   Optional
                                                                 
                                                                 
  -------------------------------------------------------------- ----------
  -------------------------------------------------------------------------

Disable Location Services
-------------------------

  ----------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Security & Privacy.*                                                                 Mandatory
                                                                                                                         
  -   Choose tab *Privacy.*                                                                                              
                                                                                                                         
  -   Uncheck *Enable Location Services* or uncheck applications which should NOT be able to access location services.   
                                                                                                                         
                                                                                                                         
  ---------------------------------------------------------------------------------------------------------------------- -----------
  ----------------------------------------------------------------------------------------------------------------------------------

Enable Firewall
---------------

  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Security & Privacy.*                                                                                                                                                           Mandatory
                                                                                                                                                                                                                   
  -   Choose tab *Firewall.*                                                                                                                                                                                       
                                                                                                                                                                                                                   
  -   Click *Turn On Firewall.*                                                                                                                                                                                    
                                                                                                                                                                                                                   
  -   Click *Firewall Options….*                                                                                                                                                                                   
                                                                                                                                                                                                                   
  -   Check *Block all incoming connections.*                                                                                                                                                                      
                                                                                                                                                                                                                   
  -   Check *Automatically allow signed software to receive incoming connections* only, if you’re not familiar with firewall configurations and you want to make sure, that all functionality will be available.   
                                                                                                                                                                                                                   
  -   Check *Enable stealth mode.*                                                                                                                                                                                 
                                                                                                                                                                                                                   
                                                                                                                                                                                                                   
  ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- -----------
  ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Disable Wake-on-LAN
-------------------

  -------------------------------------------------------------
  -   Go to *System Preferences* → *Energy Saver*   Mandatory
                                                    
  -   Choose tab *Options*                          
                                                    
  -   Uncheck *Wake for network access.*            
                                                    
                                                    
  ------------------------------------------------- -----------
  -------------------------------------------------------------

Limit IPv6 to Local Subnet/Disable IPv6[^3]
-------------------------------------------

  -------------------------------------------------------------------------------------------------------------------------------------------------------
  -   Go to *System Preferences* → *Network.*                                                                                                  Optional
                                                                                                                                               
  -   For all relevant interfaces click *Advanced….*                                                                                           
                                                                                                                                               
  -   For *Configure IPv6* select *Link-local only.*                                                                                           
                                                                                                                                               
  > This will ensure that IPv6 is only used in the local subnet. If you would like to disable IPv6 completely, enter the following commands:   
                                                                                                                                               
  -   To list all network devices: *networksetup –listallnetworkservices.*                                                                     
                                                                                                                                               
  -   To disable IPv6 on a specific network device: *networksetup -setv6off Wi-Fi*                                                             
                                                                                                                                               
                                                                                                                                               
  -------------------------------------------------------------------------------------------------------------------------------------------- ----------
  -------------------------------------------------------------------------------------------------------------------------------------------------------

1.  Logging & Monitoring
    ====================

2.  Enable BSM Audit
    ----------------

  -------------------------------------------------------------------------------------------------------------------------------------------------------------
  -   Edit /etc/security/audit\_control and include the following lines:                                                                             Optional
                                                                                                                                                     
  > dir:/var/audit                                                                                                                                   
  >                                                                                                                                                  
  > flags:all                                                                                                                                        
  >                                                                                                                                                  
  > minfree:5                                                                                                                                        
  >                                                                                                                                                  
  > naflags:lo,aa,pc,nt                                                                                                                              
  >                                                                                                                                                  
  > policy:cnt,argv                                                                                                                                  
  >                                                                                                                                                  
  > filesz:1G                                                                                                                                        
  >                                                                                                                                                  
  > expire-after:5G                                                                                                                                  
  >                                                                                                                                                  
  > superuser-set-sflags-mask:has\_authenticated,has\_console\_access                                                                                
  >                                                                                                                                                  
  > superuser-clear-sflags-mask:has\_authenticated,has\_console\_access                                                                              
  >                                                                                                                                                  
  > member-set-sflags-mask:                                                                                                                          
  >                                                                                                                                                  
  > member-clear-sflags-mask:has\_authenticated                                                                                                      
                                                                                                                                                     
  -   ![](media/image4.png){width="0.22916666666666666in" height="0.22916666666666666in"}Start a new audit trail using the adjusted configuration:   
                                                                                                                                                     
  ![](media/image3.png){width="0.19444444444444445in" height="0.19444444444444445in"} sudo audit -n                                                  
                                                                                                                                                     
  As only new processes will be audited, the system must be restarted.                                                                               
  -------------------------------------------------------------------------------------------------------------------------------------------------- ----------
  -------------------------------------------------------------------------------------------------------------------------------------------------------------

Apendix: List of Services
=========================

The following table lists service files and the corresponding
functionality that should be disabled/must not be enabled unless
required.

  **Filename**                                   **Functionality**
  ---------------------------------------------- -----------------------
  com.apple.AppleFileServer.plist                AFP
  ftp.plist                                      FTP
  smbd.plist                                     SMB
  org.apache.httpd.plist                         HTTP Server
  eppc.plist                                     Remote Apple Events
  com.apple.xgridagentd.plist                    Xgrid
  com.apple.xgridcontrollerd.plist               Xgrid
  com.apple.InternetSharing.plist                Internet Sharing
  com.apple.dashboard.advisory.fetch.plist       Dashboard Auto-Update
  com.apple.UserNotificationCenter.plist         User notifications
  com.apple.RemoteDesktop.PrivilegeProxy.plist   ARD
  com.apple.RemoteDesktop.plist                  ARD
  com.apple.IIDCAssistant.plist                  iSight
  com.apple.blued.plist                          Bluetooth
  com.apple.RemoteUI.plist                       Remote Control

[^1]: In combination with the previous line, this option does not have
    any effect, yet we recommended it in case timestamp\_timeout will be
    changed.

[^2]: This setting only enables automatic updates for the system and
    system software. Updates for 3rd party software must be installed
    manually/in another way.

[^3]: While IPv6 is not in use in many environments yet, we basically
    recommend to gather operational and security requirements for future
    deployments:\
    http://blog.ipspace.net/2013/05/the-dangers-of-ignoring-ipv6.html
