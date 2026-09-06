# riscv-cpu

A RISC-V processor implementing a subset of the RV32I base integer instruction set, written from scratch in Verilog and carried through the complete hardware flow: RTL design, FPGA implementation on a Xilinx Artix-7, and an open-source ASIC flow producing a GDSII layout.

## Status

**In progress.** Building RTL components. See the roadmap for what is complete.

## Scope

A **single-cycle** RV32I core supporting twenty-one instructions — enough to compute, branch, loop, call functions, and access memory:

| Category | Instructions |
|---|---|
| Register-register | `add` `sub` `and` `or` `xor` `slt` `sll` `srl` `sra` |
| Register-immediate | `addi` `andi` `ori` `xori` `slti` |
| Memory | `lw` `sw` |
| Branch | `beq` `bne` |
| Jump | `jal` `jalr` |
| Upper immediate | `lui` |

Byte and halfword accesses, unsigned comparisons, and `auipc` are deliberately out of scope. None of them change the datapath structure, so they can be added later without rework.

Single-cycle rather than pipelined: the goal is a complete design taken all the way to silicon layout, not maximum clock frequency. Pipelining is a natural follow-on once the flow is proven end to end.

## Repository layout

```
riscv-cpu/
├── rtl/          Verilog source: components, then the assembled core
├── tb/           Testbenches
├── programs/     Test programs in assembly and hex
├── fpga/         Constraints and board-specific top level
├── asic/         OpenLane configuration and flow outputs
└── docs/         Datapath diagrams and design notes
```

## Roadmap

### Phase 1 — RTL

- [ ] Register file — two read ports, one synchronous write port, `x0` hardwired to zero
- [ ] Arithmetic logic unit — the nine operations the subset requires
- [ ] Immediate generator — I, S, B, U and J reassembly with sign extension
- [ ] Main control and ALU control decode
- [ ] Instruction and data memory
- [ ] Datapath assembled and executing test programs

### Phase 2 — FPGA implementation

- [ ] Synthesis and place-and-route on Artix-7 (Basys 3)
- [ ] Timing closure, maximum frequency reported
- [ ] Programs running on hardware with observable output
- [ ] Resource utilization reported

### Phase 3 — ASIC flow

- [ ] Through OpenLane on the Sky130 open PDK to GDSII
- [ ] Gate-level simulation against the synthesized netlist
- [ ] Power, performance and area reported
- [ ] Layout screenshots and flow logs published

## Results

Populated as each phase completes: FPGA frequency and utilization, and post-layout PPA figures.

## Requirements

- [Icarus Verilog](https://steveicarus.github.io/iverilog/) and [GTKWave](https://gtkwave.sourceforge.net/) for simulation
- Xilinx Vivado for FPGA implementation
- [OpenLane](https://github.com/The-OpenROAD-Project/OpenLane) and the Sky130 PDK for the ASIC flow

## License

MIT — see [LICENSE](LICENSE).
