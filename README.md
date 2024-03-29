# ARO laboratoire 2 DECODE

## Questions

### Question 1
```
Sur l'image suivante on peut voir que l'incrémentation du PC fonctionne comme prévu et que la valeur actuel reprend la valeur du PC futur d'avant.

MOV r1, #5
MOV r2, #10
MOV r3, #40
MOV r4, #0
ADD r4, r1, r2
STR r4, [r3]
```
![Chronogram](Q1.png)

### Question 2
```
Sur la sortie rn_8_6_o.
```
![InstructionAdd/Sub](Q2.png)

### Question 3
```
Pour la condition c'est la sortie cond_11_8_o.

Pour l'offset c'est la sortie offset8_7_0_o.
```
![InstructionCondBranch](Q3.png)

### Question 4
```
Les 4 instructions travaillant uniquement avec des registres ont un opcode de 7 bits.

Les instructions employant un offset immédiat utilisent des opcodes à 5 bits.
```

## Etape 5.2

### Bus de contrôle
| Etape   | fetch_control_bus_o | decode_control_bus_o | reg_bank_control_bus_o | execute_control_bus_o | mem_control_bus_o | opcode_supported_unit |
| ------- | ------------------- | -------------------- | ---------------------- | --------------------- | ----------------- | --------------------- |
| STEP_1  | 0x0024              | 0x0011               | 0x0002                 | 0x04d0                | 0x0000            | sub_r_imm_o           |
| STEP_3  | 0x0020              | 0x0000               | 0x0002                 | 0x2088                | 0x000D            | ldrb_r_r_imm          |
| STEP_4  | 0x0030              | 0x0000               | 0x0002                 | 0x2008                | 0x000D            | ldrb_r_r_r            |
| STEP_7  | 0x0000              | 0x0000               | 0x0002                 | 0x0401                | 0x0000            | asr_r_r_imm           |
| STEP_8  | 0x0003              | 0x0002               | 0x0000                 | 0x1088                | 0x0000            | b_cond                |
| STEP_10 | 0x0001              | 0x0002               | 0x0000                 | 0x0888                | 0x0000            | b_incond              |

### Registres
| Etape   | R0     | R1     | R2     | R3     | R4     | SP     | LR     | PC     |
| ------- | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ |
| STEP_1  | 0x0020 | 0x001a | 0x0026 | 0xfff4 | 0x0018 | 0x0000 | 0x0000 | 0x0010 |
| STEP_3  | 0x0020 | 0x0024 | 0xfff4 | 0xfff4 | 0x0018 | 0x0000 | 0x0000 | 0x001C |
| STEP_4  | 0x0000 | 0x0024 | 0xfff4 | 0xfff4 | 0x0018 | 0x0000 | 0x0000 | 0x0020 |
| STEP_7  | 0x0002 | 0x0024 | 0x000b | 0xfff4 | 0x001c | 0x0000 | 0x0000 | 0x003a |
| STEP_8  | 0x0000 | 0x0000 | 0x0000 | 0xfff4 | 0x001c | 0x0000 | 0x0000 | 0x0042 |
| STEP_10 | 0x0000 | 0x0000 | 0x0000 | 0xfff4 | 0x001c | 0x0000 | 0x0000 | 0x0046 |

## Tableaux (pas besoin je crois mais c'est demandé de les complèter)

### Etape 2.5
| Instruction   | sel_cpsr | link_en | sel_PC | cond_en |
| ------------- | -------- | ------- | ------ | ------- |
| add_r_r_r_s   | 1        | 0       | 0      | 0       |
| add_r_r_imm_s | 1        | 0       | 0      | 0       |
| ldrb_r_r_r_s  | 0        | 0       | 0      | 0       |

### Etape 2.6
| Instruction   | sel_rs(0) | sel_rs(1) | sel_rn | sel_rd |
| ------------- | --------- | --------- | ------ | ------ |
| add_r_r_imm_s | 0         | 0         | 0      | 0      |
| mov_r_imm_s   | 1         | 0         | 0      | 1      |
| ror_r_r_s     | 1         | 0         | 1      | 0      |

### Etape 2.7
| Instruction   | reg_bank_wr |
| ------------- | ----------- |
| lsl_r_r_imm_s | 1           |
| eor_r_r_s     | 1           |
| strh_r_r_r_s  | 0           |

### Etape 2.9
| Instruction    | sel_mem | sel_data | ldr_data | byte |
| -------------- | ------- | -------- | -------- | ---- |
| and_r_r_s      | 0       | 0        | 0        | 0    |
| ldrh_r_r_r_s   | 1       | 0        | 1        | 0    |
| strb_r_r_imm_s | 1       | 1        | 0        | 1    |