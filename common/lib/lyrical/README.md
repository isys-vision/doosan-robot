# lyrical DRFL prebuilt — upstream import

This directory contains a **vendored, unmodified upstream prebuilt** of the
Doosan DRFL robot library. Neither the archive nor the DRFL headers carry any
version information of their own, so the exact state is recorded
here.

## Source

| | |
|---|---|
| Upstream repository | https://github.com/DoosanRobotics/doosan-robot2 (Doosan ROS 2 packages) |
| Branch | `lyrical` (targets Ubuntu 26.04 "resolute") |
| Commit | [`e0a476081ccc0e8e002d0b0f1da98d1d231e14a6`](https://github.com/DoosanRobotics/doosan-robot2/tree/e0a476081ccc0e8e002d0b0f1da98d1d231e14a6) — "chore: lyrical release 20261117" (2026-11-17T02:47Z)
| Imported on | 2026-11-17 |

## What was imported

Library (this directory, from upstream `dsr_common2/lib/lyrical/`):

    libDRFL.a
      md5    f1f84461cb7f4c71be19473aed3da29a
      sha256 5a43f4babd58ff1fe9d341776552120bdcec4a02895dad204da24be049d72d70

Headers (installed to `common/include/`, from upstream
`dsr_common2/include/` — same snapshot, always replace lib + headers together):

    DRFL.h         md5 9e2babe471f278de06a8d0692d1e5828
    DRFLEx.h       md5 186f7a8d41fa1099be6377dcd1a0c181
    DRFC.h         md5 12d7724e3ac3bc8f3ec4bf6bbaa5a284
    DRFS.h         md5 34972ed35ef8441cc4cc9846b6ed4129
    dsr_robot.h    md5 50c9c0d49e594c78c7d41020a8808db1
    dsr_serial.h   md5 2cfb29aaa50c82711726da96ff216745
    dsr_util.h     md5 261ceaa7bfe6e0e183eb500141a97cae

## Why this prebuilt exists here

The previously used prebuilt (`common/lib/noetic/x86_64/libDRFL.a`) was compiled
against **Poco <= 1.12** and references symbols that were removed or changed
in **Poco >= 1.13**, which made `dsr_control` fail at link time for i.e. Ubuntu 26.04-based 
systems. The lyrical prebuilt is compiled against the new Poco ABI.

`dsr_control/CMakeLists.txt` selects this directory for `ROS_DISTRO=one` and
as the default for unknown distros; kinetic/melodic/noetic still point at the
legacy `noetic` prebuilt.

## Updating

1. Download `libDRFL.a` and the headers listed above from the lyrical
   branch of doosan-robot2 (one consistent snapshot — do not mix versions).
2. Replace the library in this directory and the headers in
   `common/include/`.
3. Update this file (commit SHA, snapshot label, date, checksums).
4. Rebuild `dsr_control` and port it if the DRFL API changed; the monitoring
   callback layout is version-gated by `DRCF_VERSION` in `DRFLEx.h`
   (`_EX` vs `_EX2` structs).
