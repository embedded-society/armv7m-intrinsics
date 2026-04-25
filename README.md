# armv7m-intrinsics

Header-only C++20 wrappers for ARMv7-M instructions that are not normally emitted by a C/C++ compiler — the C++ analogue of CMSIS-Core's `__NOP()`, `__CLZ()`, `__LDREX*()`, etc.

Targets the **ARMv7-M** instruction set: Cortex-M3.

## Usage

Copy the library into your project (e.g., into `lib/`) and add it via CMake:

```cmake
add_subdirectory(lib/armv7m-intrinsics)
target_link_libraries(your_target PRIVATE armv7m-intrinsics)
```

Then include what you need:

```cpp
#include <armv7m-intrinsics/exclusive.hpp>
#include <armv7m-intrinsics/barriers.hpp>

bool try_increment(volatile uint32_t* counter) {
    uint32_t old_value = ArmCortex::asmLdrex(counter);
    if (ArmCortex::asmStrex(old_value + 1, counter) != 0) {
        ArmCortex::asmClrex();
        return false;
    }
    ArmCortex::asmDmb();
    return true;
}
```

## Contents

| File | Instructions | Notes |
|------|--------------|-------|
| `hints.hpp` | `NOP`, `YIELD`, `WFI`, `WFE`, `SEV`, `BKPT<imm>` | Hint-class instructions; `BKPT` takes an 8-bit immediate identifier |
| `barriers.hpp` | `DSB`, `DMB`, `ISB` | Memory and instruction synchronisation barriers (full system domain) |
| `interrupts.hpp` | `CPSIE i/f`, `CPSID i/f` | Enable/disable maskable exceptions (PRIMASK) and faults (FAULTMASK) |
| `svc.hpp` | `SVC<imm>` | Supervisor call with 8-bit immediate (template-encoded) |
| `reverse.hpp` | `REV`, `REV16`, `REVSH` | Byte-order reversal of word, halfword pairs, and signed lower halfword |
| `bits.hpp` | `CLZ`, `RBIT` | Count leading zeros, reverse bits |
| `exclusive.hpp` | `LDREX`, `LDREXH`, `LDREXB`, `STREX`, `STREXH`, `STREXB`, `CLREX` | Load/store-exclusive primitives for lock-free atomics |
| `saturation.hpp` | `SSAT<sat>`, `USAT<sat>` | Signed/unsigned saturation to `sat` bits (template-encoded) |

## Licence

This project is licensed under the Apache License Version 2.0.  
Copyright (C) 2026 The Embedded Society <https://github.com/embedded-society/armv7m-intrinsics>.  
See the attached [LICENCE](./LICENCE) file for more info.
