Basic 64 Bit Project Workflow
--------------------------------

1. Download Flat Assembly 64 at http://flatassembler.net/download.php

2. Write template

  ```bash

	format PE64 GUI 4.0
	entry Start

	include 'win64a.inc'

	section '.text' code readable executable

	  Start:
						   push  rbp
						   pop   rbp
						   invoke  ExitProcess,0


	section '.data' data readable writeable

	copyleft db 'Hello 64',0

	section '.idata' import data readable writeable

	library kernel32,'KERNEL32.DLL'

	include 'api/kernel32.inc'

  ```

 3. Create a batch file that automatically compiles your code

 
 ```bash

	@echo off
	SET INCLUDE=c:\tools\fasm64\include
	fasm.exe source.asm source.exe

 ```


