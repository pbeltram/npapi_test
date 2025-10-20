## Program options

**Program options for `papi_test`**

```
"-d, --device n              Device IP address. (Mandatory, 169.254.50.28)."
"-h, --host n                Host ip (Mandatory)."
"-c, --count n               Number of images to acquire, 0 -> endless loop. (Optional, default=0)."
"-p, --print                 Print console messages. (Optional, default is off)."
"-v, --verbose n             Dump n atoms of data content. (Optional, default is off)."
"-m, --decimate n            Data decimation. (Optional, 0=leave HW default min=1.0 max=16.0)."
"-l, --length n              Number of data in atoms (Optional, default=250000 min=1024 max=375000)."
"-x, --mux n                 Data mux selector (Optional, 0=default) 0-Decimated counter, 1-ADC Ch_AA, 2-ADC Ch_BB, 3-ADC Ch_AB).
```

- On Ubuntu 20.04 the binary application to use is **./lnx64/papi_test** (md5sum: 40e4c84199ef5fb94b809769724faa3d)

- On Windows 11 the binary application to use is **./win64/papi_test_64bit.exe** (md5sum: b10d75b5ae8912199b21be0b5adbfac0)

## Examples:

Capture 10 data buffers with 2048 data atoms from decimated ADC counter and then exit. Decimation is 16.0.

Print data buffer info and dump first 5 data atoms from each captured data buffer.

```
./lnx64/papi_test --device=169.254.50.28 --host=169.254.50.1 --decimate=16.0 --count=10 --length=2048 --mux=0 --print --verbose=5
```

Capture 10 data buffers with 125000 data atoms from ADC channels and then exit. Decimation is 16.0.

Print data buffer info and dump first 16 data atoms from each captured data buffer.

```
./lnx64/papi_test --device=169.254.50.28 --host=169.254.50.1 --decimate=16.0 --count=10 --length=125000 --mux=2 --print --verbose=16
```

Continuous capture of data buffers with 125000 data atoms from ADC channels. Decimation is 5.0 (network bw cca 810Mbits/sec). 

Print data buffer info and dump first 5 data atoms from each captured data buffer.
To exit press `<CTRL>-C`.

```
./lnx64/papi_test --device=169.254.50.28 --host=169.254.50.1 --decimate=10.0 --count=0 --length=125000 --mux=2 --print --verbose=5
```

Continuous capture of data buffers with 375000 data atoms from decimated ADC counter. Decimation is 4.2 (network bw cca 970Mbits/sec). 

This is minimum decimation without network saturation and will stream payload data at 119 Mbytes/sec.
Going over this network bandwidth limit, the fatal error messages `Data chn=0 Overflow.` will start to appear on device console output.

For missing UDP packets you will see on console output messages `send_resend:` requests and `on_packet_resend:` responses when missed UDP packet was received. 

To exit press `<CTRL>-C`.

```
./lnx64/papi_test --device=169.254.50.28 --host=169.254.50.1 --decimate=4.2 --count=0 --length=375000 --mux=0
```

**Program options for `papi_recorder`**

```
"-d, --device n              Device IP address. (Mandatory, 169.254.50.28)."
"-h, --host n                Host ip (Mandatory)."
"-c, --count n               Number of images to acquire, 0 -> endless loop. (Optional, default=0)."
"-p, --print                 Print console messages. (Optional, default is off)."
"-v, --verbose n             Dump n atoms of data content. (Optional, default is off)."
"-m, --decimate n            Data decimation. (Optional, 0=leave HW default min=4.2 max=16.0)."
"-l, --length n              Number of data in atoms (Optional, default=250000, range=250000 .. 375000))."
"-w, --wait                  Max number msec to wait for all packets (Optional, default=300 min=100))."
"-r, --resend                Max number resend retries for missing packets (Optional, default=2 min=1))."
"-n, --files                 Max number of 1Gbyte data files to write (Optional, default=5 min=1))."
"-x, --mux n                 Data mux selector (Optional, 0=default) 0-Decimated counter, 1-ADC Ch_AA, 2-ADC Ch_BB, 3-ADC Ch_AB).
"-o, --out dir               Write results to directory (<dir>/samples_vX.dat). (Optional, default is false. X=raw format version)."
```

- On Ubuntu 20.04 the binary application to use is **./lnx64/papi_recorder** (md5sum: aa7b64be6a50e2d9eb236f2f72a1aeb1)

- On Windows 11 the binary application to use is **./win64/papi_recorder_64bit.exe** (md5sum: 60b1b3cefbd7682a925dfe71e0a2e20b)

## Examples:

Continuous capture decimated counter data using default data buffer size of 250000 atoms (one DMA atom is 8 bytes).
Wait max two times of 1000msec for missed UDP packets to be resend.

```
./Release/papi_recorder --device=169.254.50.28 --host=169.254.50.1 --decimate=4.2 --count=0 --mux=0 --wait=1000
```

Capture 100 buffers of 250000 atoms and save data in file. Wait max two times of 1000msec for missed UDP packets to be resend.

```
./Release/papi_recorder --device=169.254.50.28 --host=169.254.50.1 --length=250000 --decimate=4.2 --count=100 --mux=0  --wait=1000 --out=.
```

Capture 8 1Gbyte files of ADC ChA and ChB data.  Wait max two times of 1000msec for missed UDP packets to be resend.

```
./Release/papi_recorder --device=169.254.50.28 --host=169.254.50.1 --length=250000 --decimate=4.2 --count=0 --mux=3 --wait=1000 --files=8 --out=.
```

If not all UDP packets are received for a data block, an ERROR message will be logged on console but incomplete data buffer will still be written in file with a hole in missing packets.
When capture is terminated statistic data are dumped on console.
