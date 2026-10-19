# RPS: Robo Parametric Standard

An open parametric risk standard for AI agents and robotics, published by the **RoboIns Association** (in formation), Zug, Switzerland.

> **Status: RPS-1 v0.1 Working Draft for public comment.** Prepared by the founders, not yet peer reviewed or tested in pilots. All thresholds and scales are provisional.

RPS gives insurers, operators, regulators and courts one shared language for the risk of autonomous systems:

- **System classes:** AI & software agents, industrial & logistics robotics, next-gen automated systems;
- **Core Parameter Matrix:** eight parameters (AUT, MAN, KIN, ENV, OVS, PRV, EVL, LOG) describing a system's risk profile;
- **Trigger library:** observable, independently confirmable incident events;
- **Oracle and evidence requirements:** attested telemetry, trusted time, M-of-N confirmation;
- **Agent Incident Record:** one record format for every incident, with a JSON schema;
- **Certification levels:** RPS Ready, RPS Verified, RPS Insured.

RoboIns is not an insurer and does not carry risk. Policies based on RPS are issued by licensed insurers.

## Contents

| File | Description |
|---|---|
| [RPS-1-v0.1.md](RPS-1-v0.1.md) | The standard, version 0.1 (working draft) |
| [MANIFESTO.md](MANIFESTO.md) | Why autonomous systems need human accountability |
| [schema/agent-incident-record.schema.json](schema/agent-incident-record.schema.json) | Agent Incident Record, JSON Schema 2020-12 |
| [examples/air-example.json](examples/air-example.json) | Fictional example record |
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to comment |

## Comment on the draft

Open an issue in this repository or write to **info@roboins.org**. Section 11 of the draft lists the questions we most want input on.

## How to cite

> Parov, D., & Yaremchuk, I. (2026). *RPS-1: Robo Parametric Standard, Version 0.1 (Working Draft).* RoboIns Association (in formation), Zug, Switzerland. https://roboins.org

## License

- Text of the standard and manifesto: [Creative Commons Attribution 4.0 International](LICENSE) (CC BY 4.0).
- JSON schema and examples: [Apache License 2.0](LICENSE-SCHEMA).

© 2026 Dimitar Parov, Illia Yaremchuk. https://roboins.org
