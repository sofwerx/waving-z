# Waving-Z

This program is able to encode and decode ITU G.9959 frames
(implemented, e.g. by the z-wave home automation protocol) using a
RTL-SDR dongle or an HackRF One (or any other radio handling raw IQ
files).

## Build

    mkdir build
    cd build
    cmake .. -DCMAKE_BUILD_TYPE=Release
    cmake --build .

## Example command lines for the EU frequency

The tools use the standard input and output to retrieve and generate
the data.

### Receive

Receive and decode z-wave packages using `rtl_srd` and `wave-in`

    $ rtl_sdr -f 868420000 -s 2000000 -g 25  - | ./wave-in -u

### Transmit

Encode and transmit z-wave packages using `wave-out` and
`hackrf_transfer`


    $ ./wave-out -p 'd6 b2 62 08 01 41 03 0d 07 25 01 00 9c' > turn_off_device_3.cs8
    $ hackrf_transfer -f 868420000 -s 2000000 -t turn_off.cs8

## Modulator details

The modulator is a simple FSK modulator. The modulator is phase
continuous using a simple trick (the sampling rate, baud rate, and
separation are in phase).

## Demodulator details

The demodulator is based on
https://github.com/andersesbensen/rtl-zwave and consists of an atan
demodulator and 2 nested state machines, the first one (`sample_sm`)
converts the samples into symbols and the second one (`symbol_sm`) the
bits into frame information and payload.
