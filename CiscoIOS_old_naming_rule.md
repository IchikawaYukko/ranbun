# Old Cisco IOS feature set naming rule

|    |     Feature      |             Remarks              |
|----|------------------|----------------------------------|
| k8 | Encryption (DES) |                                  |
| k9 | Encryption (???) |                                  |
| o  | Firewall         |                                  |
| s  | IP PLUS          |                                  |
| y  | IP BASE          |                                  |
| b  | AppleTalk        |                                  |
| n  | IPX              |                                  |
| r2 | IBM              |                                  |
| l  | Run from Flash   | Very old hardware (c1600 etc...) |
| m  | Run from RAM     |                                  |
| z  | compressed       |                                  |

## Example
* c1000-y-mz.113-11b.bin
    * c1000: Cisco 1000
    * y: IP BASE
    * mz: Run from RAM, Compressed IOS image
* c1600-k8osy-mz.123-24.bin
    * c1600: Cisco 1600
    * k8: DES
    * o: Firewall
    * s: IP PLUS
    * ~~y: IP BASE~~
    * mz: Run from RAM, Compressed IOS image
* c1600-bnr2sy-l.123-3a.bin
    * c1600: Cisco 1600
    * b: AppleTalk
    * n: IPX
    * r2: IBM
    * s: IP PLUS
    * ~~y: IP BASE~~
    * l: Run from Flash
