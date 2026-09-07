# Documentation

Reference material for this chip and for the IHP SG13CMOS5L Open-PDK it is built on.

## This chip

| Document | Contents |
| --- | --- |
| [specifications.md](specifications.md) | Top-level specifications: technology, supplies, clock, corner list, macro inventory, functional behaviour of every block. |
| [pinout.md](pinout.md) | The full 32-pad table per side, with the `chip_top` port and the role each pad carries inside `chip_core`. |
| [floorplan.md](floorplan.md) | Die and core geometry, hard-macro placement coordinates, the PDN strategy and the floorplan diagram. |

The three documents link to each other. They follow the design files. The design files do not
follow them. If you change a number in
[flow/librelane/config.yaml](../flow/librelane/config.yaml), [rtl/](../rtl/) or
[packaging/config.yaml](../packaging/config.yaml), write the new value here also.

## This flow

| Document | Contents |
| --- | --- |
| [template_flow.md](template_flow.md) | The full flow reference: directory tree, Xschem configuration, and every Makefile target. It comes from the upstream template README, and it also covers this repository's additions (`init-macro`, `init-submodule`, the `SIM` knob, the PDK pin). |

## Tools

| Document | Contents |
| --- | --- |
| [klayout/klayout_cheatsheet.md](klayout/klayout_cheatsheet.md) | KLayout for this PDK: shortcuts, the `SG13_dev` PCell list, layer roles, padframe components, the DRC and LVS menu entries. |
| [librelane/librelane_cheatsheet.md](librelane/librelane_cheatsheet.md) | LibreLane `final/` directory anatomy, non-default routing rules, the complete SG13CMOS5L via and metal-stack tables, the eight LEF/DEF orientations. |
| [verilog/](verilog/) | Verilog and SystemVerilog cheatsheets (external documents, PDK-independent). |

## PDK

| Document | Contents |
| --- | --- |
| [ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_layout_rules.pdf](ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_layout_rules.pdf) | The IHP layout rules. The authoritative source when a tool deck and a datasheet disagree. |
| [ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_process_spec.pdf](ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_process_spec.pdf) | The IHP process specification. |
| [ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_layout_cheatsheet.xlsx](ihp-sg13cmos5l-Open-PDK/sg13cmos5l_os_layout_cheatsheet.xlsx) | One sheet of formulas for wire resistance, parasitic capacitance and DC current limits. Read the caveats below before you use it. |
| [ihp-sg13cmos5l-Open-PDK/sg13cmos5l_ngspice_mc_mm_guide.md](ihp-sg13cmos5l-Open-PDK/sg13cmos5l_ngspice_mc_mm_guide.md) | How the ngspice model files fit together, what `mm_ok` and `num_sigmas` do, and which corner, Monte Carlo and mismatch runs are possible per device family. |
| [sizing/](sizing/) | gm/ID techsweep plot overviews for the LV and HV MOS devices, the data behind the sizing notebooks in [macros/inverter/scripts/sizing/](../macros/inverter/scripts/sizing/). |

## Other

[ihp-structure-proposals/](ihp-structure-proposals/) holds four proposed submission-repo layouts
for IHP shuttle chips. The four chips are an ADC, an LNA, an MCU and an op-amp. They were
contributed to the discussion about a common directory structure. They do not depend on the PDK.
They are kept here word for word from the SG13G2 template. The repository deliberately does not
track `gen_structure.py`, the script that produced them. See [.gitignore](../.gitignore).

## Provenance and caveats

The two PDK PDFs come from `libs.doc/doc/` of the SG13CMOS5L PDK. That PDK is
`ihp-sg13cmos5l @ ff32f48`, the PDK in the IIC-OSIC-TOOLS 2026.08 image. The content is
unchanged. The file names are lower case, to match the folder convention. The revisions are
**Rev. 0.1 (2025-12-08)** for the layout rules and **Rev. 0.2 (2025-12-15)** for the process
specification. The title page of each document shows its revision.

A repo copy of a PDK document goes stale without a signal. Check the revision against
`$PDK_ROOT/$PDK/libs.doc/doc/` before you let one of these documents decide a rule.

Three items come from the SG13G2 sibling of this template. Know about them:

- **The layout cheatsheet is the SG13G2 sheet.** It is byte-identical to
  `sg13g2_os_layout_cheatsheet.xlsx`. Its layer list thus still contains `M5`, `TM2` and `TV2`,
  which do not exist on this stack. The `M1`..`M4`, `TM1`, `Contact`, `Via 1/2/3` and `TV1` rows
  apply here unchanged. Ignore the other rows. Two more columns are wrong on both PDKs. The
  resistance columns use bulk-aluminium resistivity, and they are optimistic by approximately a
  factor of two against the tech LEF. The "min. Width" column lists 0.42 µm for TopMetal1, where
  the LEF says 1.64 µm. Take widths from the DRC deck. Take resistance from the tech LEF or from
  PEX. Take current limits from the [LibreLane cheatsheet](librelane/librelane_cheatsheet.md)
  tables, which are read out of `sg13cmos5l_tech.lef`.
- **The gm/ID techsweep plots are the SG13G2 sweep.** The LV and HV MOS ngspice models of this
  PDK are symbolic links into `ihp-sg13g2/libs.tech/ngspice/models/`. The devices are thus the
  same devices, and the curves are the same curves. The `.mat` lookup tables under
  [macros/inverter/scripts/sizing/data/](../macros/inverter/scripts/sizing/data/) are also
  byte-identical to the SG13G2 tables, under a SG13CMOS5L name. The figure captions inside the
  PDFs still read "SG13G2". A SG13CMOS5L-specific sweep will replace these plots when one exists.
- **The Verilog and SystemVerilog cheatsheets** are external documents with no PDK content at all.

No make target generates the content of `doc/`. A person maintains all of it.
