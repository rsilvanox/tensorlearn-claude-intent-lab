# Intent: Results-ready notification (PRD N-17)

Sources: `inputs/01-approved-prd.md` (approved 2026-09-18), `inputs/02-current-meeting.md` (discovery notes 2026-09-20). `inputs/03-superseded-request.md` is superseded history and approves nothing in this intent.

## Problem

- **I-1** The clinic receives avoidable calls from patients checking whether their results are ready. (01: "avoidable calls from patients checking whether results are ready")

## Proposed outcome

- **I-2** The patient is notified when a result is ready for their review in the existing authenticated portal. (01)
- **I-3** "Ready" means a doctor has reviewed **and** released the result. Laboratory upload alone never makes a notification eligible. (01: "A doctor must review and release the result before any notification is eligible"; 02, product owner: "'Ready' must not mean that the laboratory upload merely finished.")

### How success will be observed

- **S-1** Status-only "is my result ready?" calls decrease against a measured pre-launch baseline. They are counted separately from clinical questions, which are not a success measure. (01: "Measure reduction in results-status calls"; 02, support lead: "count status-only calls separately from clinical questions; compare against a measured baseline.")
- **S-2** Delivery failures are measured and tracked, and no failure results in duplicate patient messages. (01: "Measure reduction in ... delivery failures"; "handle delivery failures without duplicate patient messages")
- **S-3** No baseline value or numeric target is approved. Both are to be agreed during discovery. (01: "The baseline and numerical target will be agreed during discovery; none is approved yet.")

## Affected users and systems

- **I-4** Patients receive the notification and view the result in the existing authenticated portal. (01)
- **I-5** Doctors review and release results; their release is the trigger condition. (01)
- **I-6** Support staff handle results-status calls and supply the call counts for the baseline. (02, support lead)
- **I-7** Systems: the existing portal result route, the notification sender (channel not yet decided, see OPEN-1), and notification logs. (01; 02, engineering lead: "the existing portal path is adequate.")

**In scope:** notification via the existing portal result route, duplicate prevention for the same released result version, and delivery-failure handling.

## Out of scope

- **X-1** Portal redesign. (01)
- **X-2** A new patient dashboard. It is a separate project. (02, engineering lead)
- **X-3** Appointment scheduling. (01)
- **X-4** Clinical interpretation. (01)
- **X-5** Notifying third parties. (01) The referring doctor is not approved as a recipient and is tracked separately in OPEN-2.
- **X-6** Excluded from the superseded request: notifying on lab upload, putting test results in an SMS, copying the referring doctor, rebuilding the dashboard, and the 90%-in-one-week target. (03, superseded by 01)

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible. (01; 02)
- **C-2** The message only says that an update is available and directs the patient to the existing authenticated portal. No diagnosis, test result, test name, clinical measurement or treatment detail goes in the message or in notification and operational logs. (01; 02, privacy owner: "no sensitive content in messages or operational logs.")
- **C-3** Access is authenticated in the existing portal. Results are never attached, and no public result link is introduced. (01)
- **C-4** No duplicate notifications for the same released result version. Delivery failures are handled without duplicate patient messages. (01)
- **C-5** Only the patient is an approved recipient. No other recipient may be added without the owners' decision. (01; 02, clinical owner: "do not infer approval for referring-doctor notifications from the old request.")

## Open questions and owners

- **OPEN-1 Channel** (SMS, email or portal push). Owners: **product owner and privacy owner**, deciding after the consent and delivery review. (01; 02, privacy owner: "Consent and the actual channel are still under review.")
- **OPEN-2 Referring-doctor notification.** Owners: **clinical owner and privacy owner**, deciding recipient authorization. It is not approved. (01: "It is not approved by this PRD"; 02, clinical owner)

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
