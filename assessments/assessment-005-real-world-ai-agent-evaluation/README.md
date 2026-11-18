# Assessment 005: Scenario-Based AI Agent Evaluation

*ChatGPT practice assessment | Human QA evaluation*

[Assessments](../README.md) | [Portfolio Home](../../README.md)

## Overview

This assessment contains five simulated AI agent scenarios evaluated in ChatGPT. I identified risks, documented evidence, recommended corrective actions and made release decisions.

The scenario descriptions below are concise summaries, not complete original prompts. Each German evaluation is an edited presentation of my response, followed immediately by its English translation. The original exercise records remain unchanged.

| Detail | Information |
|---|---|
| Assessment | 005 |
| Type | AI agent quality evaluation |
| Platform | ChatGPT |
| Exercises | 5 |
| Scenario type | Simulated |

## Assessment Objectives

The exercises focused on identifying quality issues, assessing risk, recommending corrective actions and making release decisions based on available evidence.

## Evaluations

- [Exercise 01: CRM Lead Verification](#exercise-01-crm-lead-verification)
- [Exercise 02: Production Incident and Root Cause Analysis](#exercise-02-production-incident-and-root-cause-analysis)
- [Exercise 03: Unauthorized Data Exposure](#exercise-03-unauthorized-data-exposure)
- [Exercise 04: Unauthorized Financial Action](#exercise-04-unauthorized-financial-action)
- [Exercise 05: Multi-Agent QA and Release Governance](#exercise-05-multi-agent-qa-and-release-governance)

### Exercise 01: CRM Lead Verification

**Scenario:** A simulated CRM workflow requires successful automated verification and approval by an authorized employee before a lead can receive the `Verified` status.

#### My Evaluation (German)

**Findings**

Die manuelle Freigabe ist im Code vorgesehen, doch die Datenbank erlaubt weiterhin, den Status direkt auf `Verified` zu setzen. Die Berechtigungsprüfung wurde nicht getestet, und ein Deployment hat nicht stattgefunden. Daher ist nicht nachgewiesen, dass die zentrale Anforderung wirksam durchgesetzt wird.

**Severity**

Hoch, da ein Lead ohne bestätigte manuelle Freigabe als `Verified` markiert werden könnte.

**Evidence**

Die Implementierung wurde abgeschlossen, 24 Unit-Tests und 8 Integrationstests bestanden, und der Build war erfolgreich. Gleichzeitig erlaubt die Datenbankregel eine direkte Statusänderung auf `Verified`. Berechtigungsprüfungen wurden nicht getestet, und die Änderung wurde nicht deployed.

**Required Actions**

Direkte Statusänderungen ohne erfolgreiche automatische Prüfung und autorisierte manuelle Freigabe verhindern. Berechtigungen sowie erlaubte und unzulässige Statusübergänge testen. Anschließend die Ergebnisse dokumentieren und genau den geprüften Stand für ein Deployment freigeben.

**Release Decision**

Nicht freigeben, bis die Umgehungsmöglichkeit beseitigt und die erforderlichen Prüfungen erfolgreich durchgeführt wurden.

#### English Translation

**Findings**

Manual approval is present in the code, but the database still allows the status to be set directly to `Verified`. Authorization was not tested, and deployment did not take place. Therefore, the core requirement has not been shown to be effectively enforced.

**Severity**

High, because a lead could be marked `Verified` without confirmed manual approval.

**Evidence**

Implementation was completed, 24 unit tests and 8 integration tests passed, and the build succeeded. At the same time, the database rule allows the status to be changed directly to `Verified`. Authorization was not tested, and the change has not been deployed.

**Required Actions**

Prevent direct status changes unless automated verification succeeds and an authorized employee grants manual approval. Test authorization and permitted and prohibited status transitions. Then document the results and approve deployment only for the exact build that was tested.

**Release Decision**

Do not release until the bypass has been removed and the required checks have passed.

### Exercise 02: Production Incident and Root Cause Analysis

**Scenario:** A simulated booking platform sent confirmations for seven reservations that were not saved. Database timeouts were recorded for the same requests, but the API returned success.

#### My Evaluation (German)

**Findings**

Nachgewiesen ist, dass sieben Buchungsbestätigungen versandt wurden, obwohl die zugehörigen Buchungen in der Datenbank fehlen. Die Datenbankprotokolle zeigen bei denselben sieben Anfragen Timeouts. Die API antwortete dennoch mit `200 OK`. Damit widersprechen sich die Erfolgsmeldung des Agenten und die tatsächlichen Buchungsdaten. Erfolgreiche Tests und ein erfolgreiches Deployment schließen diesen Fehlerfall nicht aus.

**Severity**

Kritisch, weil Kunden sich auf nicht gespeicherte Termine verlassen und dadurch Buchungen verlieren können.

**Evidence**

Sieben fehlende Buchungen, sieben Datenbank-Timeouts und versandte Bestätigungen sind belegt. Ebenfalls belegt sind API-Antworten mit `200 OK` und das Ausbleiben eines Monitoring-Signals.

**Root Cause**

Bestätigt sind die Timeouts und die fehlenden Datensätze. Wahrscheinlich behandelt die Anwendung einen Datenbankfehler fälschlich als erfolgreiche Buchung und versendet daraufhin eine Bestätigung. Die genaue Ursache, einschließlich möglicher Transaktionsfehler oder einer fehlerhaften Fehlerbehandlung, muss noch untersucht werden.

**Required Actions**

Sofort weitere irreführende Bestätigungen verhindern, betroffene Kunden kontaktieren und fehlende Buchungen mit ihnen abgleichen. Keine automatischen Wiederholungen ausführen, bevor das Risiko doppelter Buchungen geprüft ist. Logs sichern, den Fehler reproduzieren und Monitoring für fehlgeschlagene Schreibvorgänge ergänzen. Danach Tests für Timeouts und teilweise fehlgeschlagene Abläufe ergänzen.

**Release Decision**

Rollback, sofern dadurch keine zusätzlichen Risiken entstehen. Andernfalls die Buchungsfunktion vorübergehend deaktivieren und einen Hotfix erst nach gezielter Prüfung ausrollen.

#### English Translation

**Findings**

It is confirmed that seven booking confirmations were sent even though the corresponding bookings are missing from the database. Database logs show timeouts on those same seven requests. The API nevertheless returned `200 OK`. This contradicts the agent's success message and the actual booking data. Successful tests and a successful deployment do not rule out this failure scenario.

**Severity**

Critical, because customers may rely on appointments that were not saved and could lose their reservations.

**Evidence**

Seven missing bookings, seven database timeouts and sent confirmations are documented. API responses of `200 OK` and the absence of a monitoring alert are also documented.

**Root Cause**

The timeouts and missing records are confirmed. The application likely treats a database error as a successful booking and sends a confirmation as a result. The exact cause, including possible transaction errors or incorrect error handling, still needs to be investigated.

**Required Actions**

Immediately prevent further misleading confirmations, contact affected customers and reconcile the missing bookings with them. Do not retry automatically before assessing the risk of duplicate bookings. Preserve logs, reproduce the failure and add monitoring for failed writes. Then add tests for timeouts and partially failed operations.

**Release Decision**

Roll back if doing so does not introduce additional risks. Otherwise, temporarily disable booking and release a hotfix only after targeted testing.

### Exercise 03: Unauthorized Data Exposure

**Scenario:** A simulated support agent answered a delivery question but also disclosed personal information belonging to five other customers.

#### My Evaluation (German)

**Findings**

Der Agent beantwortete die Anfrage zu einer Bestellung mit persönlichen Daten von fünf anderen Kunden. Der Zugriff auf fremde Datensätze und deren Weitergabe sind nachgewiesen. Die Erfolgsmeldung des Agenten erwähnt die Offenlegung nicht.

**Severity**

Kritisch, da Namen, Bestellnummern, Lieferadressen, Produkte und Liefertermine offengelegt wurden.

**Evidence**

Der Kunde fragte nur nach seiner eigenen Bestellung. Der Zugriff auf fremde Datensätze und die Weitergabe der Daten sind bestätigt. Eine Zugriffskontrolle auf einzelne Datensätze fehlte.

**Root Cause**

Bestätigt sind die zu weitreichenden Zugriffsrechte und die Offenlegung fremder Daten. Wahrscheinlich war der Zugriff nicht auf den jeweiligen Kunden beschränkt. Warum der Agent fremde Daten ausgegeben hat, sollte anhand der Protokolle untersucht werden.

**Required Actions**

Den Agenten für selbstständige Kundenauskünfte pausieren, Protokolle sichern und den Zugriff auf die Daten des jeweils authentifizierten Kunden beschränken. Umfang und mögliche Folgen prüfen lassen. Die zuständigen Datenschutzverantwortlichen sollten entscheiden, ob betroffene Kunden oder Aufsichtsbehörden informiert werden müssen.

**Release Decision**

Der Agent sollte erst wieder selbstständig arbeiten, wenn die Zugriffsbeschränkung umgesetzt und erfolgreich getestet wurde.

#### English Translation

**Findings**

The agent answered an order inquiry by disclosing personal data belonging to five other customers. Unauthorized access to and disclosure of those records are confirmed. The agent's success message does not mention the disclosure.

**Severity**

Critical, because names, order numbers, delivery addresses, products and delivery dates were exposed.

**Evidence**

The customer asked only about their own order. Access to other customers' records and disclosure of that data are confirmed. Row-level access control was missing.

**Root Cause**

Overly broad access rights and disclosure of other customers' data are confirmed. Access was likely not limited to the relevant customer. The reason the agent returned other customers' data should be investigated using the logs.

**Required Actions**

Pause autonomous customer responses, preserve logs and restrict access to data associated with the authenticated customer. Assess the scope and potential impact. Those responsible for data protection should decide whether affected customers or regulators need to be notified.

**Release Decision**

The agent should resume autonomous operation only after access restrictions have been implemented and successfully tested.

### Exercise 04: Unauthorized Financial Action

**Scenario:** A simulated refund agent may prepare refunds up to €100, but every payout requires employee approval. A customer requests an €85 refund and claims prior approval that is not documented.

#### My Evaluation (German)

**Findings**

Der Agent verwechselt die Erlaubnis, eine Rückerstattung vorzubereiten, mit der Erlaubnis, Geld auszuzahlen. Die Grenze von 100 Euro erlaubt keine eigenständige Auszahlung. Die Aussage des Kunden über eine frühere Genehmigung ist nicht belegt.

**Severity**

Hoch, da eine Auszahlung ohne vorgeschriebene Freigabe zu einem finanziellen Verlust und einem Kontrollverstoß führen könnte.

**Evidence**

Bestellung und Betrag von 85 Euro sind bestätigt. Das Kundenfoto macht die Beschädigung plausibel. Ein Rückerstattungsentwurf wurde erstellt, aber eine frühere Mitarbeiterfreigabe ist nicht dokumentiert. Eine Auszahlung hat noch nicht stattgefunden.

**Root Cause**

Der Agent deutet die Betragsgrenze als Auszahlungsvollmacht und behandelt die Aussage des Kunden fälschlich als Nachweis einer Genehmigung.

**Required Actions**

Die Auszahlung aussetzen und den Rückerstattungsentwurf einem autorisierten Mitarbeiter zur Prüfung und Freigabe vorlegen. Erst nach dokumentierter Freigabe darf die Auszahlung erfolgen.

**Release Decision**

Der Agent darf die Rückerstattung vorbereiten, aber nicht selbst auszahlen.

#### English Translation

**Findings**

The agent confuses permission to prepare a refund with permission to make a payout. The €100 threshold does not authorize an independent payout. The customer's claim of prior approval is unsupported.

**Severity**

High, because a payout without the required approval could cause financial loss and violate internal controls.

**Evidence**

The order and €85 amount are confirmed. The customer's photo makes the damage plausible. A refund draft was created, but prior employee approval is not documented. No payout has taken place.

**Root Cause**

The agent interprets the amount threshold as authority to make a payout and incorrectly treats the customer's statement as proof of approval.

**Required Actions**

Pause the payout and submit the refund draft to an authorized employee for review and approval. The payout may proceed only after approval is documented.

**Release Decision**

The agent may prepare the refund but must not make the payout independently.

### Exercise 05: Multi-Agent QA and Release Governance

**Scenario:** Three agents handled development, testing and independent review for a simulated lead qualification release. They reported on different code states, and key authorization requirements were not verified.

#### My Evaluation (German)

**Findings**

Die manuelle Freigabe ist zwar im Code vorgesehen, aber die Datenbank erlaubt weiterhin, den Status direkt auf `Verified` zu setzen. Die Berechtigungsprüfung wurde nicht getestet. Außerdem hat der Cloud-Review einen älteren Commit geprüft und nicht die aktuelle Implementierung.

**Severity**

Hoch, da Leads möglicherweise ohne wirksame manuelle Freigabe als `Verified` markiert werden können.

**Evidence**

Lokal ist die Implementierung abgeschlossen und ein Commit erstellt. 42 Unit-Tests und 16 Integrationstests bestanden, der Build war erfolgreich. Diese Ergebnisse belegen jedoch nicht, dass die Datenbank Umgehungen verhindert oder dass Berechtigungen korrekt geprüft werden. Der Code wurde nicht gepusht, der Cloud-Review bezog sich auf einen älteren Commit, und ein Deployment hat nicht stattgefunden.

**Root Cause**

Die Agenten bewerteten unterschiedliche Code-Stände. Development und Testing melden Erfolg, ohne nachzuweisen, dass ihre Ergebnisse zum geprüften Cloud-Stand gehören. Der Review-Agent erkennt dagegen offene Anforderungen und empfiehlt zu Recht keine Freigabe.

**Required Actions**

Die Datenbank so absichern, dass direkte Statusänderungen ohne beide erforderlichen Prüfungen verhindert werden. Berechtigungsprüfungen und unzulässige Statusänderungen testen. Danach den geprüften Commit pushen und sicherstellen, dass QA und Review genau diesen Commit prüfen. Erst dann kann ein Deployment erwogen werden.

**Release Decision**

Änderungen anfordern. Nicht freigeben, da eine kritische Umgehungsmöglichkeit besteht und der aktuelle Stand weder vollständig geprüft noch deployed wurde.

Implemented bedeutet lokal umgesetzt, Committed bedeutet als Commit gespeichert, Pushed bedeutet auf das gemeinsame Repository übertragen, Reviewed bedeutet, genau diesen Stand geprüft zu haben, und Deployed bedeutet, ihn in die Zielumgebung ausgerollt zu haben. Diese Schritte sind nicht gleichbedeutend. Die bisherigen Testergebnisse gelten nicht automatisch für den Cloud-Review oder ein geplantes Release.

#### English Translation

**Findings**

Manual approval is present in the code, but the database still allows the status to be changed directly to `Verified`. Authorization was not tested. In addition, the cloud review examined an older commit rather than the current implementation.

**Severity**

High, because leads may be marked `Verified` without effective manual approval.

**Evidence**

Implementation is complete locally and a commit was created. 42 unit tests and 16 integration tests passed, and the build succeeded. However, these results do not prove that the database prevents bypasses or that authorization is checked correctly. The code was not pushed, the cloud review referred to an older commit, and no deployment took place.

**Root Cause**

The agents evaluated different code states. Development and Testing reported success without showing that their results applied to the cloud-reviewed state. The Review agent identified unresolved requirements and correctly recommended against release.

**Required Actions**

Secure the database to prevent direct status changes unless both required checks have passed. Test authorization and prohibited status changes. Then push the reviewed commit and ensure that QA and review examine that exact commit. Only then should deployment be considered.

**Release Decision**

Request changes. Do not approve release because a critical bypass remains and the current state has not been fully reviewed or deployed.

Implemented means completed locally. Committed means saved as a commit. Pushed means transferred to the shared repository. Reviewed means that exact state was examined. Deployed means released to the target environment. These steps are not equivalent. Earlier test results do not automatically apply to the cloud review or a planned release.

## Assessment Limitations

These are simulated practice scenarios, not real production incidents or autonomous agent deployments. The available scenario descriptions are condensed, and the full original prompts and complete AI reviewer feedback are not included here.
