# ModifyExports
C++ concept for modifying export table names at runtime

How it works:  

We can use the routines provided by ImageHlp.h and dbghelp.h to write over the names of exported functions, which can NULL the results returned from GetProcAddress(). First we map an image of a DLL our program has loaded using the MapAndLoad routine, we then fetch the image's export directory by using the routine ImageExportDirectory with the results of our MapAndLoad. We then grab the list of exported function names by using ImageRvaToVa with ImageExportDirectory->AddressOfNames as the third parameter, telling it that we want the address (VA) of the image export directory. Now that we have the address (VA) of exported function names we can iterate over the number of names and call ImageRvaToVa on each iteration (with the RVA of the name) to acquire the address of the string/function name.  

The screencap following shows what it looks like to modify a function name at runtime: certain tools will be fooled, and this can perhaps be used in malware/evasion. We can see that the disassembler thinks MessageBoxA is located at both 0x7FFBE37B90D0 and 0x7FFBE37B9750.  

![Alt text](MessageBoxA_Duplicate.PNG?raw=true "Two Addresses for MessageBoxA")   
![Alt text](MyQueryObject.PNG?raw=true "MyQueryObject vs. NtQueryObject")  

