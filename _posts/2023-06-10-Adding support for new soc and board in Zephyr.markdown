---
layout: post
title: "Adding support for new soc and board in Zephyr"
date:   2023-06-10 12:15:18 +0530
---

<h1>Topics covered in this blog</h1>
Adding New SoC (J721E) and Board (BeagleBone AI-64) Support to Zephyr
<br>
<h2>Zephyr Configuration</h2>
              BeagleBone AI-64      -->   Board<br>
                  J721E_R5          -->   SoC<br>
                   J721E            -->   SoC series<br>
              Sitara Processors     -->   SoC Family<br>
                  Cortex-R5         -->   CPU<br>
                    ARM             -->   Architecture<br>
<br>
<h2>SoC</h3>
    To add support for a SoC, we need to create a directory and corresponding files
inside soc/<arch><soc_family>.                                                  
<br>                                                                                
For J721E:                                                                      
  soc/arm/ti_sitara/<[J721E, AM62x ..., common-files]>
<br>
<h3>Device Tree</h3>
    We should add the device tree files for the corresponding SoC in            
dts/<arch>/<device tree files>                                                  
<br>                                                                                
For J721E:                                                                      
  dts/arm/ti/<[j721e.dtsi, j721e_r5.dtsi]>                                      
<br>                                                                                
To create the Kconfig* files for the SoC, you can take already implementated SoC
as reference.
<br>
<h3>Linker Script</h3>
    The linker script required for the build should be added inside the         
soc/arm/ti_sitara/j721e/linker.ld. For J721E_r5, has a Cortex-R5 core.
So including zephyr/arch/arm/aarch32/cortex_a_r/scripts/linker.ld
<br>
<h3>Necessary Functions and Definitons</h3>
SYS_CLOCK_HW_CYCLES_PER_SEC<br>                                                     
    This option specifies the frequency of the hardware timer used for the system clock (in Hz)
    <br>                                                                            
sys_clock_elapsed<br>                                                               
    Returns number of ticks elapsed since the last timer interrupt.             
 <br>                                                                               
Interrupt Controller (Other than GIC):<br>
    If the SoC has a Custom Interrupt Controller other than GIC, then few function
definitions related to interrupt handling should be added.
<br>                                                                                
void z_soc_irq_init(void);<br>
void z_soc_irq_enable(unsigned int irq);<br>
void z_soc_irq_disable(unsigned int irq);<br>
int z_soc_irq_is_enabled(unsigned int irq);<br>
void z_soc_irq_priority_set(unsigned int irq, unsigned int prio, unsigned int flags);<br>
unsigned int z_soc_irq_get_active(void);<br>
void z_soc_irq_eoi(unsigned int irq);<br>
<br>                                                                                
In J721e_r5, VIM is used for handling interrupts.
<br>
<h2>Board</h2>
    To add support for a board, we need to create a board directory under boards/<arch>/<board-specific><br>
<br>
In case of BeagleBone AI-64: boards/arm/beaglebone-ai64/
<br>
<h3>Creating defconfig</h3>
CONFIG_<soc_series><br>
CONFIG_<soc><br>
# If the image is loaded and executed from flash CONFIG_XIP=y, else n<br>
CONFIG_XIP=<[<y>/<n>]<br>
CONFIG_FLASH_SIZE=0<br>
CONFIG_FLASH_BASE_ADDRESS=0<br>
CONFIG_BOOTLOADER_SRAM_SIZE=0<br>
<br>
For J721E,<br>
CONFIG_SOC_SERIES_TI_J721E=y<br>
CONFIG_SOC_TI_J721E_R5=y <br>                                                       
             <br>                                                                   
\# enable uart  <br>                                                                 
CONFIG_SERIAL=y       <br>                                                          
                          <br>                                                      
\# enable console        <br>                                                        
CONFIG_CONSOLE=y          <br>                                                      
CONFIG_UART_CONSOLE=y     <br>                                                      
                          <br>                                                      
CONFIG_BUILD_OUTPUT_UF2=n  <br>                                                     
CONFIG_BUILD_OUTPUT_HEX=y   <br>                                                    
                            <br>                                                    
CONFIG_XIP=n                <br>                                                    
CONFIG_FLASH_SIZE=0         <br>                                                    
CONFIG_FLASH_BASE_ADDRESS=0 <br>                                                    
                              <br>                                                  
CONFIG_BOOTLOADER_SRAM_SIZE=0 <br>                                                  
                              <br>                                                  
CONFIG_PRINTK=y<br>
<br>
To create other files for the board, you can take already implementated board
directory as reference.
 <br>                                                                               
Once the files are added, To buid the hello world application west command is used
> west build -p auto -b <board_name> samples/hello_world                        
                                               <br>                                 
Note: board_name is the name of the dts file in the board directory (without the .dts extension)
<br>
<h2>References</h2>
https://docs.zephyrproject.org/3.1.0/hardware/porting/board_porting.html<br>
