A passive, low-cost OT security monitor for shared renewable-energy parks.

Research and prototype work for Cognton AI's OT-security product track. The target environment is a large solar/wind park where many developers run their own plants, with mixed vendors, unmanned inverter control rooms (ICRs), and SCADA-driven remote operation.

---

## Problem

- Each plant uses different vendors, different equipment and different protocols.
- ICRs are remote and mostly unmanned, so rogue devices and physical tampering go unnoticed.
- Legacy protocols (Modbus, IEC 60870-5-104, DNP3) carry no authentication — any device that can talk can command.
- Vendor remote access (VPN, TeamViewer, 4G dongles) is poorly controlled and rarely recorded.
- Commercial OT security tools are expensive per site, and links to remote plants are slow.

## Goal

- discovers every device in an ICR from traffic alone,
- learns normal behaviour and flags deviations,
- never injects packets into control traffic,
- runs on low-cost edge hardware over a slow link.

## Targets

| Target | Measure |
| --- | --- |
| Asset discovery | Finds every device in a test ICR |
| Detection rate | ≥ 95% of simulated attacks |
| False positives | < 5% |
| Detection time | ~15 minutes |
| Response support | ~1 hour |
| Control-traffic impact | Zero (passive only) |
| Link | Works over a constrained WAN link |

---

## Missions

| ID | Mission | Deliverable |
| --- | --- | --- |
| M1 | Draw the plant | Reference architecture diagram + device/protocol table, every link labelled with its Purdue level |
| M2 | Listen to the machines | Annotated captures showing a Modbus read vs write and an IEC-104 command, plus which protocols authenticate |
| M3 | Study real attacks | Case studies mapped to MITRE ATT&CK for ICS, plus 8–10 solar-park attack scenarios used as test cases |
| M4 | Hunt for data | Comparison table of public ICS datasets and a recommendation |
| M5 | Build a mini-ICR | Dockerised plant simulation, passive sensor, live device inventory, alert on unknown device |

Missions M6–M10 (lab results, remote-access design, edge-box design, compliance and playbook, competitor and business analysis) are referenced in the submission structure and are pending scope confirmation.

---

## Tech stack

**Simulation**
- OpenPLC / pymodbus — simulated inverter and RTU
- lib60870 — IEC 60870-5-104 server
- ScadaBR or FUXA — operator HMI
- Docker Compose — orchestration

**Monitoring**
- Zeek with CISA ICSNPP parsers — ICS protocol decoding
- Suricata — signature-based detection
- Malcolm (CISA/INL) — reference packaging
- Wireshark — manual packet analysis

**Analysis and tooling**
- Python — device inventory, baselining, alerting

**Edge hardware (planned)**
- Single-board computer fed by a SPAN/mirror port or network TAP

---

## Getting started

```bash
git clone <repo-url>
cd solar-ot-security/lab
docker compose up -d
```

> The sensor is passive by design. It must only receive from a mirror port or TAP and must never transmit onto the control network.

---

## Standards and references

- NIST SP 800-82 Rev. 3 — Guide to OT Security
- ISA/IEC 62443 — zones, conduits and security levels
- MITRE ATT&CK for ICS
- CEA Cyber Security Guidelines for the Power Sector (India)
- CERT-In incident reporting directions
- SANS ICS resources and the ICS Kill Chain paper
- Dragos Year-in-Review reports

---

## Status and known limits

- No access to a live plant; all findings so far come from simulation and public data.
- Public OT datasets are "normal" traffic, so attacks are self-generated.
- Proprietary vendor protocols and encrypted vendor tunnels cannot be fully decoded.
- Field conditions (heat, dust, power cuts, slow links) are not yet modelled.

Findings are labelled throughout as **tested** or **read**.

---
