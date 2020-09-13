![Create new chip](https://github.com/JuliProg/K9F2G08U0C/workflows/Create%20new%20chip/badge.svg?event=repository_dispatch)
![ChipUpdate](https://github.com/JuliProg/K9F2G08U0C/workflows/ChipUpdate/badge.svg)
# Join the development of the project ([list of tasks](https://github.com/users/JuliProg/projects/1))


# K9F2G08U0C
Implementation of the K9F2G08U0C chip for the JuliProg programmer

Dependency injection, DI based on MEF framework is used to connect the chip to the programmer.

<section class = "listing">

#
```c#

    public class ChipAssembly
    {
        [Export("Chip")]
        ChipPrototype myChip = new ChipPrototype();
```
# Chip parameters
```c#


        ChipAssembly()
        {
            myChip.devManuf = "SAMSUNG";
            myChip.name = "K9F2G08U0C";
            myChip.chipID = "ECDA101544";      // device ID - ECh DAh 10h 15h 44h (K9F2G08UOC-SCBO_C40316.pdf page 28)

            myChip.width = Organization.x8;    // chip width - 8 bit
            myChip.bytesPP = 2048;             // page size - 2048 byte (2Kb)
            myChip.spareBytesPP = 64;          // size Spare Area - 64 byte
            myChip.pagesPB = 64;               // the number of pages per block - 64 
            myChip.bloksPLUN = 2048;           // number of blocks in CE - 2048
            myChip.LUNs = 1;                   // the amount of CE in the chip
            myChip.colAdrCycles = 2;           // cycles for column addressing
            myChip.rowAdrCycles = 3;           // cycles for row addressing 
            myChip.vcc = Vcc.v3_3;             // supply voltage

```
# Chip operations
```c#


            //------- Add chip operations ----------------------------------------------------

            myChip.Operations("Reset_FFh").
                   Operations("Erase_60h_D0h").
                   Operations("Read_00h_30h").
                   Operations("PageProgram_80h_10h");

```
# Chip registers (optional)
```c#


            //------- Add chip registers (optional)----------------------------------------------------

            myChip.registers.Add(
                "Status Register").
                Size(1).
                Operations("ReadStatus_70h").
                Interpretation("SR_Interpreted").   //From https://github.com/JuliProg/Wiki/wiki/Status-Register-Interpretation
                UseAsStatusRegister();



            myChip.registers.Add(
                "Id Register").
                Size(5).
                Operations("ReadId_90h").               
                Interpretation(ID_interpreting);          // From here

```
# Interpretation of ID-register values ​​(optional)
```c#


        public string ID_interpreting(Register register)   
        
```
</section>







footer
