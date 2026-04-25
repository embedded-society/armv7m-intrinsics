# armv7m-intrinsics

Header-only C++20 wrappers for ARMv7-M instructions that are not normally emitted by a C/C++ compiler — the C++ analogue of CMSIS-Core's `__NOP()`, `__CLZ()`, `__LDREX*()`, etc.

Targets the **ARMv7-M** instruction set: Cortex-M3.

For ARMv6-M (Cortex-M0/M0+/M1), see [armv6m-intrinsics](https://github.com/embedded-society/armv6m-intrinsics).
For ARMv7E-M / DSP (Cortex-M4), see [armv7em-intrinsics](https://github.com/embedded-society/armv7em-intrinsics).

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

CMake is optional — every header is self-contained, so you can drop the `include/` directory anywhere on your include path and `#include` directly without using CMake at all.

## Contents

All wrappers live in the `ArmCortex` namespace and are `[[gnu::always_inline]] static inline` so the compiler inlines them straight into the call site without function-call overhead.

This library is a **superset** of the ARMv6-M instruction set, since ARMv7-M includes everything in ARMv6-M.

### Inherited from ARMv6-M

| File | Instructions | Notes |
|------|--------------|-------|
| `hints.hpp` | `NOP`, `YIELD`, `WFI`, `WFE`, `SEV`, `BKPT<imm>` | Hint-class instructions |
| `barriers.hpp` | `DSB`, `DMB`, `ISB` | Memory and instruction synchronisation barriers |
| `interrupts.hpp` | `CPSIE i`, `CPSID i` | Enable/disable maskable exceptions via PRIMASK |
| `svc.hpp` | `SVC<imm>` | Supervisor call with 8-bit immediate |
| `reverse.hpp` | `REV`, `REV16`, `REVSH` | Byte-order reversal |

### Added in ARMv7-M

| File | Instructions | Notes |
|------|--------------|-------|
| `interrupts.hpp` adds | `CPSIE f`, `CPSID f` | FAULTMASK control (mask all faults except NMI) |
| `bits.hpp` | `CLZ`, `RBIT` | Count leading zeros, reverse bits |
| `exclusive.hpp` | `LDREX`, `LDREXH`, `LDREXB`, `STREX`, `STREXH`, `STREXB`, `CLREX` | Load/store-exclusive primitives for lock-free atomics |
| `saturation.hpp` | `SSAT<sat>`, `USAT<sat>` | Signed/unsigned saturation to `sat` bits (template-encoded) |

## Compatibility

Designed to be a drop-in replacement for the ARMv7-M-relevant subset of the deprecated `ARMCortexM-CppLib::intrinsics` headers. Function names and signatures follow the existing `asmFoo()` convention.

## Licence

This project is licensed under the Apache License Version 2.0.  
Copyright (C) 2026 The Embedded Society <https://github.com/embedded-society/armv7m-intrinsics>.  
See the attached [LICENCE](./LICENCE) file for more info.
