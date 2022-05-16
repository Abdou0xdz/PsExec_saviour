# PsExec_saviour
Solutions to "access is denied" using PsExec



Hi, i am posting a summary from many sources online for various solutions to "access is denied" : most information can be found here (including requirements needed) - sysinternal help

    as someone mentioned add this reg key, and then restart the computer :

        reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\system /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1 /f

    Read this knowledge base article to learn what this does and why it is needed

    Disable firewall (note - this will leave you with out any firewall protection)

        netsh advfirewall set allprofiles state off

    if target user has a blank PW and you dont want to add one, run on target:

        [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa] "LimitBlankPasswordUse"=dword:00000000

    This didnt work for me, but i have read it did for others in a few places, on target execute:

        Start -> Run -> secpol.msc -> Local Policies -> Security Options -> Network Access: Sharing > and security model for local accounts > Classic – local users authenticate as themselves

    if already in 'Classic':

        move to "Guest only - .." run from elevated command prompt gpupdate \force move back to 'Classic - .." again run from elevated command prompt gpupdate \force

    This one solved my issue:

        run on target from elevated command prompt "net use" look at ouput chart and for shares listed in remote column there (i only deleted the disconnected ones - you can try them all) run "net use [remote path from before list] /delete" then run 'net use \target\Admin$ /user:[user name]' enter prompt password request (if empty PW just press enter), viola should work.

good luck, I hope that this will save someones time.
