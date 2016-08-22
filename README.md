# redsea

redsea is a lightweight command-line utility that decodes
[RDS](http://en.wikipedia.org/wiki/Radio_Data_System) data from FM broadcasts
and prints it to the terminal. It works well with rtl_fm.

[explanatory blog post](http://www.windytan.com/2015/02/receiving-rds-with-rtl-sdr.html)

## Compiling

You will need [liquid-dsp](https://github.com/jgaeddert/liquid-dsp) and GNU autotools.

```
autoreconf --install
./configure
make
```

If you get an error message about the STDCXX_11 macro, try installing `autoconf-archive`.

## Usage

### Live decoding with rtl_fm

```
./rtl-rx.sh -f 87.9M
```

The `-f` parameter sets the station frequency. Note that `rtl_fm` will tune a bit off; this is expected behavior.

### Decoding a pre-recorded signal with SoX

```
sox multiplex.wav -t .s16 -r 228k - | ./src/redsea
```

The signal should be FM demodulated and have enough bandwidth to accommodate the RDS subcarrier (> 60 kHz).

### Decoding serial data

Raw serial RDS bits can be fed into the program as ASCII `0`s and `1`s using the `-b` option:

```
./src/redsea -b < bits.txt
```

## Requirements

* Linux/OSX
* C++11 compiler
* GNU autotools
* [liquid-dsp](https://github.com/jgaeddert/liquid-dsp)
* rtl_fm (from [rtl-sdr](http://sdr.osmocom.org/trac/wiki/rtl-sdr)) or any other source that can output demodulated FM multiplex signals

## Features

Redsea decodes the following info from RDS:

* Program Identification code (PI)
* Program Service name (PS)
* Radiotext (RT)
* Traffic Program (TP) and Traffic Announcement (TA) flags
* Program Type (PTY)
* Alternate Frequencies (AF)
* Clock Time and Date (CT)
* Program Item Number (PIN)
* Traffic Message Channel (TMC)

Output format is currently line delimited JSON.

## Licensing

```
Copyright (c) Oona Räisänen OH2EIQ (windyoona@gmail.com)

Permission to use, copy, modify, and/or distribute this software for any
purpose with or without fee is hereby granted, provided that the above
copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
```
