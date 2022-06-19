# PE / Executable Packer
 Simple software packer for x32 and x64 executable
 
 <img src="https://github.com/NRicode/PE-Executable-Packer/blob/main/demo/screenshot.png" width= "auto" height= "200px"></img>
 
# Notice
This PE packer **DOES NOT encrypt** the payload to prevent misuse. This is simply a POC of a PE loader that can load & execute another executable in the same address space as the loader. Usually this is what people call "Crypter", a tool that can encrypt another malware to bypass static analysis. This can also be improved to have anti VM, anti debugging or anti analysis feature.

# My findings
When I test different PE with this loader. I found that PE with static base address (commonly `0x400000`), has some problems. These static PE does not have `IMAGE_DIRECTORY_ENTRY_BASERELOC` in the Data Directory structure. What this mean is there is no way to load these PE at any other location other than at it's desired base address. Other PE loader deal with this by loading the PE at it's desired base address and all of them works maybe 80% of the time. The other 20% is when the memory allocated at that address overlap with the stub's base address and result in access violation. Loading at the base address also startle all antivirus as they can compare the PE header in memory and the PE header on the disk. So far I don't find any good ways to fix this other than using some dirty hacks that can also lead to crashes. and I have yet to find any other PE loader successfuly fix this problem.

# How it works
1. Memory map the PE
2. Fix base relocation values
3. Fix imports
4. Fix PEB
