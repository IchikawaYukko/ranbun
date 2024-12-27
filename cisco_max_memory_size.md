# Cisco router's max memory size information

## Cisco 1600
16MB PCMCIA Linear Flash

## Cisco 2800 (2811)
2GB CF (FAT16)

If insert 4GB, negative capacity will be appear on show flash: command.
```
CallManagerExpress#sh fla
No files on device

-185270272 bytes available (0 bytes used)

2GB is OK!!
CallManagerExpress#sh fla
-#- --length-- -----date/time------ path
1     36805116 Dec 27 2024 11:24:12 +09:00 c2800nm-advipservicesk9-mz.124-10a.bin

2042068992 bytes available (36831232 bytes used)
```

## Cisco 3600 (3620 / 3640 / 3660)
20??MB Linear Flash

# Extra info
## Sun Fire V120
Main Memory: 4GB ECC SDRAM (1GB x 4)