# Driver\_-\_TDR315H
Driver for interfacing with the Acclima TDR-315H soil moist sensor

### Error Response

Device has two aspects of error reporting (in addition to general SDI-12 and sensor errors)

The first reports error _flags_, the second reports error _data_ (if applicable). Both use the same base report code

**`TDR315_ERROR`** -> `0xA0020000`

The sensor errors are described in detail in the [manual](https://acclima.com/wp-content/uploads/Acclima-TDR-Sensor-User-Manual-REV-8-25-2021.pdf), an error is thrown for each individual error which is reported by the sensor 

Example:

Error Code -> `0xA0020512`

Indicates an error code of 'High Conductivity Error' on sensor connected to Talon 1, Port 2

Below is a listing of fault descriptions, type, and corresponding codes

| Bit Position | Description | Error Code |
| --------- | ----------- | ---------- |
| 0 | Incident wave starts high | `0xA00200tp` |
| 1 | Incident wave does not rise | `0xA00201tp` |
| 2 | Idle voltage error | `0xA00202tp` |
| 3 | Active voltage error | `0xA00203tp` |
| 4 | Temperature out of range | `0xA00204tp` |
| 5 | High conductivity error | `0xA00205tp` | 
| 6 | Measurement math error | `0xA00206tp` |

**Note:** `t` and `p` indicate insertion of Talon value and Port value respectively 

Some of these error codes also pass information in the form of 'Error Data', this information is encoded into a special error report

Error Data Report: `0xA00280tp` 
	The highest bit of the sub-type is set to indicate the passing of error data, the sub-type is broken down as follows `0b1vvvvvvv`, where `v` indicates the values of error data. Since error data must have a value between 0 and 99, it can be encoded into these last 7 bits of the error code

### Interface Errata

#### Extended Identity `zXI!`

As of firmware version `40.3.0.0` the sensor response to the extended identity call does not match that of documentation (Dated Sept. 30, 2021)

Expected: `aFwVer=vv.v.v.v,HwVer=x,MfgDate(D/M/Y)=dd/mm/yy`
Actual Result: `0Fw=40.3.0.0,Hw=H3,MfgDMY=12/7/22,SN=5008173`

Note the discrepancies in naming etc, this is relevant for parsing and so documented here