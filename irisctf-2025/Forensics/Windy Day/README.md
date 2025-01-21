# Windy Day

## Challenge Description

![Description](./assets/chall.png)

## Solution

We are given a memory dump for this challenge. So we got to our beloved volatility for the analysis.

### Initial Analysis

I started analyzing the memdump.mem using [Volatility3](https://github.com/volatilityfoundation/volatility3).

The first step was as always listing the processes.

```bash
w0lf@hp:~/volatility3$ python3 vol.py -f memdump.mem windows.pslist
Volatility 3 Framework 2.6.1
Progress:  100.00               PDB scanning finished
PID     PPID    ImageFileName   Offset(V)       Threads Handles SessionId       Wow64   CreateTime      ExitTime        File output

4       0       System  0xe38cb727f6c0  98      -       N/A     False   2025-01-03 16:50:11.000000      N/A     Disabled
260     4       smss.exe        0xe38cb79de040  2       -       N/A     False   2025-01-03 16:50:11.000000      N/A     Disabled
364     356     csrss.exe       0xe38cb7aa2440  9       -       0       False   2025-01-03 16:50:12.000000      N/A     Disabled
428     260     smss.exe        0xe38cb7d79380  0       -       1       False   2025-01-03 16:50:12.000000      2025-01-03 16:50:12.000000      Disabled
436     428     csrss.exe       0xe38cb7d95340  11      -       1       False   2025-01-03 16:50:12.000000      N/A     Disabled
444     356     wininit.exe     0xe38cb7d9a080  1       -       0       False   2025-01-03 16:50:12.000000      N/A     Disabled
488     428     winlogon.exe    0xe38cb7980080  4       -       1       False   2025-01-03 16:50:12.000000      N/A     Disabled
548     444     services.exe    0xe38cb7f8c080  5       -       0       False   2025-01-03 16:50:13.000000      N/A     Disabled
556     444     lsass.exe       0xe38cb7f89080  7       -       0       False   2025-01-03 16:50:13.000000      N/A     Disabled
628     548     svchost.exe     0xe38cb7fdc4c0  17      -       0       False   2025-01-03 16:50:13.000000      N/A     Disabled
672     548     svchost.exe     0xe38cb83ed180  10      -       0       False   2025-01-03 16:50:13.000000      N/A     Disabled
780     488     dwm.exe 0xe38cbae30080  12      -       1       False   2025-01-03 16:50:13.000000      N/A     Disabled
888     548     svchost.exe     0xe38cbaeac500  46      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
896     548     svchost.exe     0xe38cbaeb8340  19      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
904     548     svchost.exe     0xe38cbaebd800  23      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
948     548     svchost.exe     0xe38cbaee1800  12      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
964     548     svchost.exe     0xe38cbaeea800  18      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
624     548     svchost.exe     0xe38cbaf45800  24      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
1076    548     svchost.exe     0xe38cb82032c0  20      -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
1132    548     svchost.exe     0xe38cb824f800  6       -       0       False   2025-01-03 16:50:14.000000      N/A     Disabled
1360    548     svchost.exe     0xe38cb82eb080  6       -       0       False   2025-01-03 16:50:15.000000      N/A     Disabled
1696    548     svchost.exe     0xe38cb83d3800  11      -       0       False   2025-01-03 16:50:16.000000      N/A     Disabled
1776    548     svchost.exe     0xe38cb8006800  5       -       0       False   2025-01-03 16:50:16.000000      N/A     Disabled
1784    548     svchost.exe     0xe38cb800f800  9       -       0       False   2025-01-03 16:50:16.000000      N/A     Disabled
1832    548     MsMpEng.exe     0xe38cb83e6340  27      -       0       False   2025-01-03 16:50:16.000000      N/A     Disabled
2676    548     NisSrv.exe      0xe38cbb0bf800  3       -       0       False   2025-01-03 16:50:23.000000      N/A     Disabled
2144    628     RuntimeBroker.  0xe38cb81a2080  12      -       1       False   2025-01-03 16:50:59.000000      N/A     Disabled
2208    548     svchost.exe     0xe38cbb1de080  7       -       1       False   2025-01-03 16:51:00.000000      N/A     Disabled
2220    888     sihost.exe      0xe38cbb1f8800  10      -       1       False   2025-01-03 16:51:00.000000      N/A     Disabled
2276    888     taskhostw.exe   0xe38cb8140080  11      -       1       False   2025-01-03 16:51:00.000000      N/A     Disabled
2592    488     userinit.exe    0xe38cbb202080  0       -       1       False   2025-01-03 16:51:01.000000      2025-01-03 16:51:31.000000      Disabled
2856    2592    explorer.exe    0xe38cbb239800  70      -       1       False   2025-01-03 16:51:01.000000      N/A     Disabled
2064    628     ShellExperienc  0xe38cbb2a4800  28      -       1       False   2025-01-03 16:51:03.000000      N/A     Disabled
2216    628     SearchUI.exe    0xe38cbb2e3800  16      -       1       False   2025-01-03 16:51:04.000000      N/A     Disabled
3248    2312    ServerManager.  0xe38cbb3d3800  13      -       1       False   2025-01-03 16:51:06.000000      N/A     Disabled
3472    628     dllhost.exe     0xe38cb75b3340  2       -       1       False   2025-01-03 16:52:20.000000      N/A     Disabled
3464    548     msdtc.exe       0xe38cb75c7800  9       -       0       False   2025-01-03 16:52:21.000000      N/A     Disabled
1604    488     fontdrvhost.ex  0xe38cb75d8080  5       -       1       False   2025-01-03 16:54:00.000000      N/A     Disabled
3036    4060    firefox.exe     0xe38cb818b500  89      -       1       True    2025-01-03 16:55:40.000000      N/A     Disabled
3968    3036    firefox.exe     0xe38cb75b9080  22      -       1       True    2025-01-03 16:55:41.000000      N/A     Disabled
3624    3036    firefox.exe     0xe38cbb380080  5       -       1       True    2025-01-03 16:55:41.000000      N/A     Disabled
3828    3036    firefox.exe     0xe38cbb539800  17      -       1       True    2025-01-03 16:55:43.000000      N/A     Disabled
2420    3036    firefox.exe     0xe38cbb711800  5       -       1       True    2025-01-03 16:55:44.000000      N/A     Disabled
4076    3036    firefox.exe     0xe38cbb116080  17      -       1       True    2025-01-03 16:55:45.000000      N/A     Disabled
3132    3036    firefox.exe     0xe38cbb1e1080  5       -       1       True    2025-01-03 16:55:47.000000      N/A     Disabled
712     628     ApplicationFra  0xe38cbb573080  1       -       1       False   2025-01-03 16:56:02.000000      N/A     Disabled
5044    3036    firefox.exe     0xe38cbb7c8800  5       -       1       True    2025-01-03 16:56:43.000000      N/A     Disabled
4772    3036    firefox.exe     0xe38cbb0d6300  18      -       1       True    2025-01-03 16:57:38.000000      N/A     Disabled
1380    2856    FTK Imager.exe  0xe38cbb82e800  20      -       1       False   2025-01-03 17:02:19.000000      N/A     Disabled
5324    3036    firefox.exe     0xe38cbba94080  19      -       1       True    2025-01-03 17:09:52.000000      N/A     Disabled
5804    3036    firefox.exe     0xe38cbb261080  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
2308    3036    firefox.exe     0xe38cbbb44080  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
2712    3036    firefox.exe     0xe38cbba16080  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
3328    3036    firefox.exe     0xe38cbb8c6800  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
6256    3036    firefox.exe     0xe38cbb884800  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
5288    3036    firefox.exe     0xe38cbb8a9800  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
5664    3036    firefox.exe     0xe38cbb7c9080  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
5196    3036    firefox.exe     0xe38cbb8c0800  19      -       1       True    2025-01-03 17:09:56.000000      N/A     Disabled
4508    3036    firefox.exe     0xe38cbbbe8800  19      -       1       True    2025-01-03 17:09:59.000000      N/A     Disabled
4344    3036    firefox.exe     0xe38cbbbb8080  19      -       1       True    2025-01-03 17:10:00.000000      N/A     Disabled
928     3036    firefox.exe     0xe38cbbc8c080  19      -       1       True    2025-01-03 17:10:00.000000      N/A     Disabled
740     3036    firefox.exe     0xe38cbb89e400  19      -       1       True    2025-01-03 17:10:01.000000      N/A     Disabled
2252    3036    firefox.exe     0xe38cb7e25080  19      -       1       True    2025-01-03 17:10:01.000000      N/A     Disabled
2920    3036    firefox.exe     0xe38cbbc9e080  19      -       1       True    2025-01-03 17:10:01.000000      N/A     Disabled
3124    3036    firefox.exe     0xe38cbbca0080  19      -       1       True    2025-01-03 17:10:01.000000      N/A     Disabled
7116    3036    firefox.exe     0xe38cb7e32680  19      -       1       True    2025-01-03 17:10:01.000000      N/A     Disabled
6344    628     smartscreen.ex  0xe38cbbcd9080  13      -       1       False   2025-01-03 17:10:06.000000      N/A     Disabled
7292    7052    MpCmdRun.exe    0xe38cbbad5800  5       -       0       False   2025-01-03 17:10:10.000000      N/A     Disabled
8136    8112    Taskmgr.exe     0xe38cbb713800  15      -       1       False   2025-01-03 17:10:50.000000      N/A     Disabled
4124    628     WmiPrvSE.exe    0xe38cbba9e800  10      -       0       False   2025-01-03 17:11:13.000000      N/A     Disabled
5628    628     WmiPrvSE.exe    0xe38cb7d80080  9       -       0       False   2025-01-03 17:11:13.000000      N/A     Disabled
```

This tells us that at the time of memory capture `firefox.exe` had been running and also since it only happens to be any process to concern, so the next step was to dump the memory of the proces firefox.exe (PID: 3036)

```bash
$python3 vol.py -f memdump.mem windows.memmap --pid 3036 --dump
```
Now I had my firefox process dump and since its a browser so the most sensible thing to search for was URLs but that happened to be too many so now it was just some trial and error until we get the result that it was a google URL that happened to encode our flag in base64.

```bash
..
.
https://www.google.com/search?client=firefox-b-d&q=aXJpc2N0ZntpX2FtX2FuX2lkaW90X3dpdGhfYmFkX21lbW9yeX0%3D
.
```

### Decoding our Flag

```bash
$echo "aXJpc2N0ZntpX2FtX2FuX2lkaW90X3dpdGhfYmFkX21lbW9yeX0=" | base64 -d
irisctf{i_am_an_idiot_with_bad_memory}
```

Flag: 
```yaml
irisctf{i_am_an_idiot_with_bad_memory}
```

### Cheesy Approach
After the CTF ended the discussion led to the relevation that just the string search the base64 encoding of `irisct` could yield this URL and hence the flag as well.

```bash
w0lf@hp:~/volatility3$ strings memdump.mem | grep aXJpc2N0
https://www.google.com/search?client=firefox-b-d&q=aXJpc2N0Z
https://www.google.com/complete/search?client=firefox&channel=fen&q=aXJpc2N0ZntpX2FtX2FuX2l
https://www.google.com/search?client=firefox-b-d&q=aXJpc2N0Z
https://www.google.com/search?client=firefox-b-d&q=aXJpc2N0ZntpX2FtX2F
https://www.google.com/complete/search?client=firefox&channel=fen&q=aXJpc2N0Zn
https://www.google.com/complete/search?client=firefox&channel=fen&q=aXJpc2N0Zn
O^firstPartyDomain=google-b-d.search.suggestions.mozilla,a,::https://www.google.com/complete/search?client=firefox&channel=fen&q=aXJpc2N0ZntpX2FtX2FuX2
https://www.google.com/search?client=firefox-b-d&q=aXJpc2N0ZntpX2FtX2FuX2
.
.
client=firefox-b-d&q=aXJpc2N0ZntpX2FtX2FuX2lkaW90X3dpdGhfYmFkX21lbW9yeX0%3D
client=firefox-b-d&q=aXJpc2N0ZntpX2FtX2FuX2lkaW90X3dpdGhfYmFkX21lbW9yeX0%3D
.
..
```
