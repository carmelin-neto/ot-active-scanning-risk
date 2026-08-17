# IT/OT Segmentation & Scanning Risk — Why OT Networks Require Different Security Tooling Than IT

## Objective
Explain, with technical justification, why standard IT security practices — 
active vulnerability scanning, aggressive patch cadence, penetration testing 
tools — can be unsafe when applied directly to operational technology (OT) 
environments, and what safer alternatives look like.

## Why This Project
Demonstrating this judgment is more valuable at entry-level than demonstrating 
exploitation skill. A candidate who understands why you don't run a standard 
Nmap scan against a live PLC is more hireable in critical infrastructure than 
one who only knows how to run scanning tools against a lab VM. This is a 
companion piece to my [Purdue Model Mapping project](https://github.com/carmelin-neto/ot-purdue-model-mapping) 
— that project defines the network boundaries; this one explains why crossing 
them carelessly is dangerous.
*Related project: [ICS Modbus Attack Testing](https://github.com/carmelin-neto/ics-modbus-attack-testing) — hands-on testing that puts these scanning-risk concepts into practice.*

## What I Did
Researched documented cases of active scanning tools causing failures in live 
PLC environments, and wrote up the underlying reasoning for why OT and IT 
security practices diverge — grounded in verified technical sources, not 
assumption.

## Findings

A standard vulnerability scan works by sending fast, high-volume traffic to a 
device to figure out what it is and what's running on it. In IT, that's 
routine and low-risk. In OT, it's not.

Industrial PLCs and RTUs run simple, real-time operating systems. They're 
built to run control logic, not to handle a flood of unexpected network 
traffic. This isn't just theory — it's documented. In one test, running Nmap 
against a live PLC caused it to enter a failure state: its status lights 
started flashing, and it needed a full power cycle to recover. The 
researchers noted this could happen by accident to anyone using Nmap 
normally — no special skill or bad intent required. In another test, Nessus 
crashed a PLC mid-scan. Running a normal IT scan against a live controller is 
like throwing a wrench into a fragile machine — the controller just isn't 
built to absorb that kind of hit.

This is why OT security and IT security prioritize differently. IT puts 
confidentiality first — keeping data secret. OT puts availability first — 
keeping the plant running and safe. A scan that might expose data in IT can 
freeze a controller in OT. If that controller runs something like a conveyor 
belt, a valve, or a safety system, a crash doesn't just mean downtime. It can 
stop production instantly or take a safety system offline.

Because of this, active scanning is avoided in live OT environments. The 
safer option is passive network monitoring — listening to a copy of the 
network traffic instead of sending anything to the devices directly. This 
lets you see what's on the network and how it talks, without ever touching a 
live controller. Nothing gets sent, so nothing can crash.

## What I'd Do Differently in Production
This writeup is desk research and conceptual analysis. A real OT risk 
assessment would require direct collaboration with control systems engineers, 
vendor-specific documentation, and validated testing procedures rather than 
general research findings — I don't yet have access to any of those.

## Sources
- PLC vulnerability scanning research (ScienceDirect) — documented Nessus 
  scan causing PLC crash during testing
- ICS asset discovery tools taxonomy (arXiv) — documented Nmap scan causing 
  PLC failure state requiring power cycle
