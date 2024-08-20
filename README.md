# Automated Parking Management System

## Overview

This project involves the design of a device for managing a parking lot with automated entry and exit. The parking lot is divided into three sectors: Sectors A and B each have 31 parking spaces, while Sector C has 24 parking spaces. Upon entry, the user must declare the sector they wish to park in, and similarly, upon exit, they must declare the sector they are leaving.

### System Initialization

- The parking lot remains open during the night, allowing all vehicles to enter and exit freely.
- In the morning, the device is manually activated by an operator using a 5-bit sequence `11111`.
- On the next clock cycle, the system waits for the input of the number of cars present in Sector A (in 5 bits) and stores the value.
- The same process occurs for Sectors B and C in the subsequent two cycles.
- If the value entered exceeds the number of parking spaces in the respective sector, all spaces in that sector are considered occupied.
- Starting from the fourth clock cycle, the system begins autonomous operation.

### System Operation

In each cycle, a user approaches the entrance or exit and presses a button corresponding to the sector they intend to park in. The circuit has two inputs defined as follows:

- **IN/OUT [2 bits]**: When a user is entering, the system receives the input `01`; when exiting, it receives `10`. Inputs `00` and `11` are interpreted as system anomalies, and the request is ignored (i.e., no gate is opened).
- **SECTOR [3 bits]**: Sectors are indicated using one-hot encoding, with a 3-bit string where only one bit is set to 1 and the others to 0. The encoding is as follows: `100` for Sector A, `010` for Sector B, and `001` for Sector C. Any other encoding is interpreted as an input error, and the request is ignored.

### Outputs

The device has three outputs in the following order:

1. **SECTOR_NON_VALIDO [1 bit]**: This bit is set if the sector input is invalid.
2. **SBARRA_(IN/OUT) [1 bit]**: This bit is `0` if the gate is closed, and `1` if the gate is opened. The gate remains open for only one clock cycle and then closes, even if the next request is invalid.
3. **SECTOR_(A/B/C) [1 bit per sector]**: This bit is `1` if all parking spaces in the sector are occupied, and `0` if there are still free spaces.

### System Shutdown

The device shuts down when it receives the input sequence `00000`. If an invalid sector is entered, the gate remains closed, and the state of the sectors is still updated according to their current situation.


