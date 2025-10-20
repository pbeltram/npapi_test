**papi_check_data**

Script will read captured data and perform some checks on captured data.
For captured DataType=0 (ADC decimated counter) it will verify that `diff(succ-pred) == 1` for all captured data.
By default scripts plots only first 200000 data samples (or less if length of captured data is shorter).

Parameters:

```
-i : input file (required, default None).
-h : Help.
```

Run python script on Ubuntu:

```
./papi_check_data.py -i ./samples_v1.dat
```

Run python script on Windows 11:

```
python.exe .\papi_check_data.py -i .\samples_v1.dat
```
