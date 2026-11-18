# Process Models

This folder contains the business process models of the claims management process.

## Deliverables

| File | Content | Tool | Status |
|---|---|---|---|
| `claim-handling-process.bpmn` + `.png` | BPMN 2.0 model of the end-to-end claim process with swimlanes: **Customer**, **Supervisor**, **Agent**, **System**. | Bizagi Modeler / Camunda Modeler / draw.io | To do |
| `flowchart-open-claim.png` | Flowchart of the "Open claim" use case (validations, duplicate check BR-03, priority and SLA calculation). | draw.io | To do |
| `flowchart-sla-calculation.png` | Flowchart of the SLA due-date algorithm (business days + holidays + API fallback). | draw.io | To do |
| `flowchart-status-change.png` | Flowchart of a status change (permission check, transition validation, history record). | draw.io | To do |

## BPMN checklist
- [ ] Start event: *Customer reports a problem*
- [ ] Exclusive gateway: *Is the claim valid?* → reject / continue
- [ ] Task: *Calculate priority and SLA* (System lane, service task)
- [ ] Task: *Assign agent* (Supervisor lane)
- [ ] Task: *Diagnose and solve* (Agent lane)
- [ ] Gateway: *Needs customer information?* → message flow to Customer
- [ ] Timer boundary event: *SLA due date reached* → escalate to supervisor
- [ ] Timer event: *72 h without reply* → close automatically
- [ ] End events: *Claim closed* / *Claim rejected*

> Tip: export each diagram as PNG and embed it in the main README and in Confluence.
