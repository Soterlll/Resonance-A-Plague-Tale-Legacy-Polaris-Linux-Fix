# Credits and licensing

This is a patch against **vkd3d-proton v3.1** (https://github.com/HansKristian-Work/vkd3d-proton),
licensed under **LGPL-2.1-or-later**. The patch and the binaries built from it are distributed under the same
license; the full corresponding source is the upstream tag v3.1 plus `patch/RX580-resonance-v3.1.patch`.

The Polaris compatibility approach (forged capability reporting, FP16 handling, ExecuteIndirect emulation,
watchdog) is based on **njtechyt/RX580-Crimson-Desert-Fix-Linux-Only** (Apache-2.0), which in turn builds on the
scheme of **Vidith007/RX-580-CRIMSON-DESERT-FIX**. The analysis of what Resonance needs on Polaris draws on the
Windows fix by **ibr3himkhaled** (Resonance-A-Plague-Tale-Legacy-AMD-Polaris-Fix). No code or shader binaries from
that Windows fix are included here.

Unofficial and unaffiliated with Valve, Asobo, Focus or the vkd3d-proton authors.
