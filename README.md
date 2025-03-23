# Enable_Semihosting_In_Stm32F103
Enable Semi hosting in Stm32F103 

Here is a structured guide in English to enable Semihosting on STM32F103 using STM32CubeIDE and OpenOCD

Instructions:

Step 1: Build the Project to Generate the Binary

Step 2: Enable OpenOCD in Debugger Configuration
	Right click project -> Select Debug Configuration -> Go to "Debugger" tab -> Navigation "Debug probe" -> Select "ST link Open OCD "
 	![alt text](image.png)

Step 3: Add Semihosting Run Command
	In the Debug Configurations window, go to the Startup tab.
 	Locate " Run Command " Section
  	Add command " monitor arm semihosting enable "
![alt text](image-1.png)

Step 4: Add Linker Flags for Semihosting
	1. Right-click on your project in Project Explorer.
 	2. Navigate to Properties > C/C++ Build > Settings.
  	3. Under MCU GCC Linker > Miscellaneous, add the following flags:
   		"-specs=rdimon.specs -lc -lrdimon"
![alt text](image.png)

Step 5: Modify main.c to Enable Semihosting.
Open main.c and add the necessary function declarations.
Modify main.c as follows:

	#include <stdio.h>

	extern void initialise_monitor_handles(void);  // Enable semihosting

	void init_semihosting(void) {
		initialise_monitor_handles();
		printf("Semihosting enabled!\n");
	}

	int main(void) {
		init_semihosting();  // Initialize semihosting

		while (1) {
			printf("Running...\n");
		}
	}

Step 6: Exclude syscalls.c to Avoid Conflicts
Why?

	The default syscalls.c file implements functions like _write() for printf, which conflicts with semihosting.

How to exclude it?

	1. Right-click on the syscalls.c file inside the Src folder.
	2. Select Resource Configurations > Exclude from Build.
	3. Tick both Debug and Release configurations.
	4. Click OK.

![alt text](image-2.png)


Note:

If impletement all instruction above and met error like this:

	TARGET: STM32F103C8Tx.cpu - Not halted
	Error: Target not halted
	Error: failed erasing sectors 0 to 4
	Error: flash_erase returned -304
	Info : dropped 'gdb' connection
	shutdown command invoked
	Info : dropped 'gdb' connection

Solution:
	While press "debugger" -> press reset   
Why?
	To target halted
