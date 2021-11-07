# asrock_z390_taichi
[Lm-sensors](https://github.com/lm-sensors/lm-sensors) configuration for [ASRock Z390 Taichi](https://www.asrock.com/mb/Intel/Z390%20Taichi/index.asp) motherboards on Linux.

**Warning:** this configuration is hardware dependent, can work properly only on this motherboard! You have to modify the configuration if you want to use it on different hardware. All vendors and motherboards are using SIO chips differently. 

## The motherboard
ASRock installed two SIO  (Super I/O) chips on Z390 Taichi motherboard to handle multiple fans and voltage lines.
 - primary SIO is **Nuvoton NCT6791D**
 - secondary SIO is **Nuvoton NCT5567D**

 <img src="https://static.tweaktown.com/content/8/7/8755_36_asrock-fatal1ty-z390-taichi-intel-motherboard-preview.jpg" align="center" width="500">
 
See more details in the [review of tweaktown.com](https://www.tweaktown.com/articles/8755/asrock-z390-taichi-intel-motherboard-preview/index.html).
 Note: this is not a typical setup, usually one SIO chip can handle everything.

## lm-sensors
The `sensors-detect` command in the `lm-sensors` package can identify these SIO chips:

    Some Super I/O chips contain embedded sensors. We have to write to
    standard I/O ports to probe them. This is usually safe.
    Do you want to scan for Super I/O sensors? (YES/no): 
    Probing for Super-I/O at 0x2e/0x2f
    Trying family `National Semiconductor/ITE'...               No
    Trying family `SMSC'...                                     No
    Trying family `VIA/Winbond/Nuvoton/Fintek'...               Yes
    Found `Nuvoton NCT6791D Super IO Sensors'                   Success!
        (address 0x290, driver `nct6775')
    Probing for Super-I/O at 0x4e/0x4f
    Trying family `National Semiconductor/ITE'...               No
    Trying family `SMSC'...                                     No
    Trying family `VIA/Winbond/Nuvoton/Fintek'...               Yes
    Found `Nuvoton NCT6793D Super IO Sensors'                   Success!
        (address 0xa10, driver `nct6775')

and the `sensors` command is able to report their status (if the kernel module `nct6775` is loaded):

    nct6791-isa-0290
    Adapter: ISA adapter
    in0:                   648.00 mV (min =  +0.08 V, max =  +0.74 V)
    in1:                     1.67 V  (min =  +1.50 V, max =  +1.83 V)
    in2:                     3.47 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in3:                     3.47 V  (min =  +2.98 V, max =  +3.63 V)
    in4:                     1.01 V  (min =  +0.95 V, max =  +1.05 V)
    in5:                     1.06 V  (min =  +0.94 V, max =  +2.04 V)
    in6:                     1.06 V  (min =  +0.95 V, max =  +1.50 V)
    in7:                     3.49 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in8:                     3.22 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in9:                     1.06 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in10:                  232.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in11:                    1.22 V  (min =  +0.85 V, max =  +2.00 V)
    in12:                    1.37 V  (min =  +1.00 V, max =  +2.04 V)
    in13:                    1.27 V  (min =  +1.20 V, max =  +1.40 V)
    in14:                    1.27 V  (min =  +0.95 V, max =  +2.04 V)
    fan1:                   458 RPM  (min =    0 RPM)
    fan2:                   527 RPM  (min =  200 RPM)
    fan3:                     0 RPM  (min =    0 RPM)
    fan4:                   700 RPM  (min =    0 RPM)
    fan5:                   489 RPM  (min =    0 RPM)
    SYSTIN:                 +34.0°C  (high = +60.0°C, hyst = +35.0°C)  sensor = thermistor
    CPUTIN:                 -63.0°C  (high = +36.0°C, hyst = +35.0°C)  sensor = thermistor
    AUXTIN0:                +23.0°C    sensor = thermistor
    AUXTIN1:                +99.0°C    sensor = thermistor
    AUXTIN2:                +15.0°C    sensor = thermistor
    AUXTIN3:                +12.0°C    sensor = thermistor
    PECI Agent 0:           +31.0°C  (high = +36.0°C, hyst = +35.0°C)
                                     (crit = +115.0°C)
    PCH_CHIP_CPU_MAX_TEMP:   +0.0°C  
    PCH_CHIP_TEMP:           +0.0°C  
    PCH_CPU_TEMP:            +0.0°C  
    intrusion0:            ALARM
    intrusion1:            ALARM
    beep_enable:           enabled
    ...
    nct6793-isa-0a10
    Adapter: ISA adapter
    in0:                   192.00 mV (min =  +0.00 V, max =  +1.74 V)
    in1:                   944.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in2:                     3.47 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in3:                     3.31 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in4:                   256.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in5:                   136.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in6:                   840.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in7:                     3.46 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in8:                     3.20 V  (min =  +0.00 V, max =  +0.00 V)  ALARM
    in9:                     0.00 V  (min =  +0.00 V, max =  +0.00 V)
    in10:                  128.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in11:                  136.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    in12:                  944.00 mV (min =  +0.94 V, max =  +2.04 V)
    in13:                    1.04 V  (min =  +0.86 V, max =  +2.02 V)
    in14:                  176.00 mV (min =  +0.00 V, max =  +0.00 V)  ALARM
    fan1:                   523 RPM  (min =    0 RPM)
    fan2:                     0 RPM  (min =    0 RPM)
    fan3:                     0 RPM  (min =    0 RPM)
    fan4:                     0 RPM  (min =    0 RPM)
    fan5:                     0 RPM  (min =    0 RPM)
    SYSTIN:                +114.0°C  (high =  +0.0°C, hyst =  +0.0°C)  sensor = thermistor
    CPUTIN:                 +32.0°C  (high = +90.0°C, hyst = +75.0°C)  sensor = thermistor
    AUXTIN0:                +35.0°C  (high =  +0.0°C, hyst =  +0.0°C)  ALARM  sensor = thermistor
    AUXTIN1:               +107.0°C    sensor = thermistor
    AUXTIN2:               +107.0°C    sensor = thermistor
    AUXTIN3:               +107.0°C    sensor = thermistor
    PCH_CHIP_CPU_MAX_TEMP:   +0.0°C  
    PCH_CHIP_TEMP:           +0.0°C  
    PCH_CPU_TEMP:            +0.0°C  
    PCH_MCH_TEMP:            +0.0°C  
    intrusion0:            OK
    intrusion1:            ALARM
    beep_enable:           enabled

NOTE: `lm-sensors` recognized the secondary NCT5567D SIO chip as NCT6793, but it seems to work properly (TODO: contact the author of nct6775 on this issue).

## How to identify `lm-sensors` entities?
This is the hardest part since motherboard and SIO chip vendors are not always providing documentation about their implementation details. 
I found [this project](https://gist.github.com/aaronsb/347d62b63456ae131916c3affd212c05), where Aaron Bockelie shared a good hint on how to identify `lm-sensor` entities in case of ASRock motherboards. He suggested to use [Nuvoton NCT677xF Data Sheet](https://media.digikey.com/pdf/data%20sheets/nuvoton%20pdfs/nct6776f,d.pdf) and the configuration file of [ASRock A-Tuning utility](https://www.asrock.com/mb/Intel/Z390%20Taichi/index.asp#Download) (check this file: `C:\Program Files (x86)\ASRock Utility\A-Tuning\Conf\Z390TAICHI.xml`).

Using these sources and doing some further investigations I managed to identify the following entities on this motherboard.

### 1. voltages
|Motherboard | SIO | lm-sensor | NCT reference | NCT description| ASRock utility | Min | Max |
|--|--|--|--|--|--|--|--|
| Vcore | NCT6791D | in0 | index80 | CPUVCORE | CPU_INPUT_V | 0.16 | 1.48 |
| +5V | NCT6791D | in1 | index81 | VIN1 | P5P0_V | 4.5 | 5.5 |
| ignored | NCT6791D | in2 | index82 | AVSB | none |  |  |
| +3.3V | NCT6791D | in3 | index83 | 3VCC | P3P3_V | 2.98 | 3.63 |
| CPU PLL2 | NCT6791D | in5 | index85 | n/a | SIO6_V | 0.936 | 2.613 |
| PCH +1.0V | NCT6791D | in6 | index86 | VIN4 | SIO3_V | 0.95 | 1.5 |
| ignored | NCT6791D | in7 | index87 | 3VSB | none | | |
| ignored | NCT6791D | in8 | index88 | VBAT | none | | |
| ignored | NCT6791D | in9 | index89 | VTT | none | | |
| ignored | NCT6791D | in10 | index8a | VIN5 | none | | |
| VCCIO | NCT6791D | in11 | index8b | VIN26 | SIO4_V | 0.84 | 2.0 |
| DRAM | NCT6791D | in12 | index8c | VIN2 | SIO1_V | 1.0 | 2.3 |
| DRAM VPP | NCT6791D | in13 | index8d | VIN3 | SIO2_V | 2.4 | 2.8 |
| VCCSA | NCT6791D | in14 | index8e | VIN7 | SIO5_V | 0.95 | 2.0 |
| Cold Bug Killer | NCT5567D | in12 | index8c | VIN2 | SIO7_V | 0.936 | 2.613 |
| DMI | NCT5567D | in13 | index8d | VIN3 | SIO8_V | 0.864 | 2.016 |

Mapping and naming of the different voltage lines can be identified with the help of A-Tuning's configuration file. If you run this utility then the minimum and maximum voltage values will be displayed.

### 2. fans
|Motherboard | SIO | lm-sensor |
|--|--|--|
| Chassis fan3 | NCT6791D | fan1 |
| CPU fan1 | NCT6791D | fan2 |
| CPU fan2 | NCT6791D | fan3 |
| Chassis fan1 | NCT6791D | fan4 |
| Chassis fan2 | NCT6791D | fan5 |
| Chassis fan5 | NCT5567D | fan1 |
| Chassis fan6 | NCT5567D | fan2 |
| ignored | NCT5567D | fan3 |
| ignored | NCT5567D | fan4 |
| Chassis fan4 | NCT5567D | fan5 |

These assignments were checked manually on the motherboard. Using only one fan in different  connector and checking the `lm-sensor` report will make this step straightforward. Luckily the fan values do not require additional computation here.

### 3. temperatures
|Motherboard | SIO | lm-sensor |
|--|--|--|
| Motherboard | NCT6791D | temp1 (SYSTIN) |
| CPU | NCT5567D | temp2 (CPUTIN) |

Please note that CPU sensor is located on the motherboard and it always shows lower temperature than the core temperature of the CPU (use `coretemp` module if you need internal temperature values). On the other hand I did not find information about the meaning of the following entities (NCT6791D: AUXTIN0/PECI Agent 0, NCT5567D: AUXTIN0) they are ignored actually.

## The new configuration
You can copy this new configuration file (`asrock_z390_taichi.conf`) to folder `/etc/sensors.d/` and the output of the `sensors` command will look like this:

    nct6791-isa-0290
    Adapter: ISA adapter
    Vcore:          1.30 V  (min =  +0.16 V, max =  +1.49 V)
    +5V:            5.02 V  (min =  +4.51 V, max =  +5.50 V)
    +3.3V:          3.47 V  (min =  +2.98 V, max =  +3.63 V)
    +12V:          12.10 V  (min = +11.42 V, max = +12.58 V)
    CPU PLL2:       1.06 V  (min =  +0.94 V, max =  +2.04 V)
    PCH +1.0V:      1.06 V  (min =  +0.95 V, max =  +1.50 V)
    VCCIO:          1.22 V  (min =  +0.85 V, max =  +2.00 V)
    DRAM:           1.37 V  (min =  +1.00 V, max =  +2.04 V)
    DRAM VPP:       2.53 V  (min =  +2.40 V, max =  +2.80 V)
    VCCSA:          1.27 V  (min =  +0.95 V, max =  +2.04 V)
    Chassis fan3:  439 RPM  (min =    0 RPM)
    CPU fan1:      522 RPM  (min =  200 RPM)
    CPU fan2:        0 RPM  (min =    0 RPM)
    Chassis fan1:  681 RPM  (min =    0 RPM)
    Chassis fan2:  470 RPM  (min =    0 RPM)
    Motherboard:   +34.0°C  (high = +60.0°C, hyst = +35.0°C)  sensor = thermistor
    beep_enable:  enabled
    ...
    nct6793-isa-0a10
    Adapter: ISA adapter
    Cold Bug Killer: 944.00 mV (min =  +0.94 V, max =  +2.04 V)
    DMI:               1.04 V  (min =  +0.86 V, max =  +2.02 V)
    Chassis fan5:     517 RPM  (min =    0 RPM)
    Chassis fan6:       0 RPM  (min =    0 RPM)
    Chassis fan4:       0 RPM  (min =    0 RPM)
    CPU:              +31.5°C  (high = +90.0°C, hyst = +75.0°C)  sensor = thermistor
    beep_enable:     enabled

Notes:

 - Beep warnings are enabled.
 - There is a limitation how sensors can handle voltage values above 2.048V (read more about this topic on page 59 in [NCT677xF Data Sheet](https://media.digikey.com/pdf/data%20sheets/nuvoton%20pdfs/nct6776f,d.pdf)). I assume this is the reason why some maximum values are truncated at 2.04V in `lm-sensors`.
 - All fans are enabled in the configuration. You can use `ignore` statement to hide any of them.
 - If you modify min or max values in the configuration you have to restart the systemd service of `lm-sensors` (e.g. `systemctl restart lm_sensors.service`).
 - This configuration was tested on Debian Linux 11 and Arch Linux.

## References

 - github: [lm-sensors project](https://github.com/lm-sensors/lm-sensors)
 - [nct6775 kernel module](https://www.kernel.org/doc/html/v5.12/hwmon/nct6775.html)
 - [Nuvoton NCT677xF Data sheet](https://media.digikey.com/pdf/data%20sheets/nuvoton%20pdfs/nct6776f,d.pdf)
 - [Tweaktown.com review of ASRock Z390 Taichi motherboard](https://www.tweaktown.com/articles/8755/asrock-z390-taichi-intel-motherboard-preview/index.html)
 - github: [aaronsb](https://gist.github.com/aaronsb)/[ASRock_Z390m.conf](https://gist.github.com/aaronsb/347d62b63456ae131916c3affd212c05)
 - [Improving Nuvoton NCT6776 lm_sensors output](https://blog.hqcodeshop.fi/archives/276-Improving-Nuvoton-NCT6776-lm_sensors-output.html)

> Written with [StackEdit](https://stackedit.io/).


