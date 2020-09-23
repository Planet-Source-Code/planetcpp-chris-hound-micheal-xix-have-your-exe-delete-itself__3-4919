<div align="center">

## Have your Exe Delete Itself


</div>

### Description

Whether your making a setup application, an uninstaller, or for whatever reason,

your probably going to be faced with the problem of self deletion. How can you delete

your own EXE thus leaving no traces of your program on the computer? You can't use another

program that you've written, ..then how do you delete that EXE?

It turns out that we can rely on good ol' windows to help us commit Hari Kari, but the

technique is different on NT based machines. First I'll show you the code for 9x/ME machines.

For those systems you need to add an entry into wininit.ini, this is a initialization file that

windows will use on boot. This file resides in your windows directory but might not be present

on your computer since it gets renamed to WININIT.BAK after each use. This doesn't concern us

since the WritePrivateProfileString function will create whatever is missing, that is the function

used to write data to ini files. What you add is the string:

NUL=yourexe.exe

With the path of course, into the [RENAME] section . After this is called the file will be deleted

next time you reboot your machine. Here is the code:
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[PlanetCPP\-Chris\(Hound\)/Micheal\(xix\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/planetcpp-chris-hound-micheal-xix.md)
**Level**          |Intermediate
**User Rating**    |3.5 (14 globes from 4 users)
**Compatibility**  |C\+\+ \(general\), Microsoft Visual C\+\+, Borland C\+\+
**Category**       |[Files](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/files__3-2.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/planetcpp-chris-hound-micheal-xix-have-your-exe-delete-itself__3-4919/archive/master.zip)





### Source Code

```
#include <iostream>
#include <windows.h>
using namespace std;
int main()
{
	char shortpath[MAX_PATH] = {""},longpath[MAX_PATH] = {""};
	HMODULE modhandle;
	modhandle = GetModuleHandle(NULL);	//Get module handle of our program
	GetModuleFileName(modhandle,longpath,MAX_PATH);	//Get long path and filename
	//You cannot use the long path and filename, you have to convert them to short
	GetShortPathName(longpath,shortpath,MAX_PATH);	//Convert to short path version
	WritePrivateProfileString("RENAME","NUL",shortpath,"WININIT.INI");	//add to ini file
	cout<<"i've lived a good life, but my time is done here ;o(\n";	//say goodbye
	cout<<"Goodbye... (reboot now to finish me off)\n";
  return 0;
}
 /*
If you thought this couldn't get any easier...your wrong, on NT machines it's a simple
one-liner, all you need to use is the MoveFileEx function. This function has a flag called
MOVEFILE_DELAY_UNTIL_REBOOT, this adds the action into the registry key
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\
Session Manager\PendingFileRenameOperations
and performs them the next time you reboot. All you need to do is tell it to move your file to NULL.
MoveFileEx(MyExe, NULL, MOVEFILE_DELAY_UNTIL_REBOOT);
And that's it! Use the path and filename of your program for MyExe of course.
*/
```

