# IDAPython_Scripts
Either general IDAPython scripts I made to aid me in everyday reversing or IDAPython scripts I made targeting a specific executable (e.g. deobfuscate a specific routine that is unlikely to appear in other executables). All IDAPython scripts I made from now on will be placed here. 

### CCCheck ###
The 0xCC byte is the byte representing int 3, or software breakpoint. When you make a software breakpoint on an instruction, the debugger replaces the first byte of the instruction to 0xCC. When the CPU hits the int 3 instruction, the OS will signal SIGTRAP to the debugged program. But since the program is being debugged, the debugger will catch it instead, effectively halting the execution temporatory. The 0xCC byte can also be added to the program itself by the original software developers to thwart off people trying to reverse engineer their program since running the program under a debugger will stop it at random 0xCC instructions. This script checks the .text section for the 0xCC bytes and prints the addresses of where the 0xCC bytes are located if they exist. Being able to quickly identify where all the manually added 0xCC bytes are makes the initial dynamic analysis process smoother. 

### Deobfuscate ###
Deobfuscates a portion of the code and data for a crackme by Tosh. This script will directly patch the bytes in IDA so IDA will show the correct deobfuscated listing rather than writing the deobfuscated listing to a separate file. This enhances static analysis and makes solving this crackme challenge a lot faster. Full write-up of this particular crackme can be viewed on my blog (http://yellowbyte.blogspot.com/2017/01/elf-anti-debug-root-me-cracking.html).

### FindMain ###
In a stripped ELF executable, IDA will not be able to identify main. The name of the main function will be indistinguishable from the other local functions, in the form sub_"address of where it is located." This script will automatically find and rename main as "main" and then move cursor position in IDA's disassembly listing to beginning of main. This script currently only works for GNU Compiler Collection (GCC) compiled ELF executables.

### JccFlip ###
Change a jcc instruction to its opposite representation. For example, JA to JB. This is helpful when one is patching a binary to bypass the binary's authorization routine. 

### LocFuncAnalyzer ###
In a stripped ELF binary, local functions are deprived of its original name. This is why local functions are not usually the starting point when doing analysis since without its original name, all local functions look exactly the same as one another. This script aims to change that. For each local function, it prints out the numbers of code references, data references, and arguments. You might be wondering how those information may be helpful. Well, a large number of code references is suspicious. For example, in a binary where readable texts are obfuscated, the text de-obfuscator function will have a large number of code references. The numbers of data references is also helpful to know since having any number of data references can mean that the function is being called indirectly, which is suspicious. Knowing the numbers of arguments for a local function can also be helpful. For example, if you are solving a crackme and there is a local function that takes two arguments, that function could be the authentication function that checks your input against the correct input. Being able to identify the authentication function quickly makes it easier to solve the crackme. 

### MalCheck ###
Checks an executable for usage of API that has a high chance of being used maliciously or for anti-reversing purposes such as IsDebuggerPresent. It's always a good idea to check for low-hanging fruits before doing any deeper analysis. The "potentially malicious" functions that I came up with are from the book "Practical Malware Analysis."

### NopSled ###
Either convert the instructions that user select/highlight or the instruction that the mouse cursor is on to NOPs. This can be useful to clean up Anti-Reversing techniques that add in dead/useless code to make it harder to analysis the disassembly.  

### RdtscCheck ###
rdtsc instruction puts the number of ticks since the last system reboot in EDX:EAX. There is really no point for a binary to contain this instruction other than for anti-debugging purpose. To use as a debugging deterrent, program will have this instruction at at least two different places in the .text section and then have a compare instruction that compares the time eclipsed between two rdtsc instructions. If a breakpoint is placed anywhere between two rdtsc instructions, then the time eclipsed between the two instructions will be significantly higher, signaling that it is running under a debugger. This script checks the .text section for the rdtsc instructions and print out their addresses if they exist. 
