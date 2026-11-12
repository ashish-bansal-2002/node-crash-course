Ordering-address vendors stay extended to the purchasing org only. Direct procurement from a domestic delivering entity was raised as a new scenario and needs business confirmation. Invoicing parties exist only for payment and bank accounts.

| Item | Detail |
| --- | --- |
| Date | Not stated in transcript (follow-up to the 9 Nov call) |
| Topic | WHT follow-up: vendor extension, invoicing party, exemption fields, certificates |
| Participants (as heard) | Ashish, Harshita, Sourabh (both dropped early), Andy, Nikki, Kranti |
| Mentioned | Shrikant (testing), Saravana (vendor onboarding), Sushil, Kaushal, Nikita (off today), Ajay Prasad |

**Key outcomes**

- Ordering-address (ODA) vendors are extended to the purchasing org only, not the company code. This is the existing load process. **Closed.**
- Direct procurement by Singapore from a domestic entity (e.g. PwC Singapore) is new to the project team. It needs business confirmation before any design or data change.
- Invoicing parties (IVPs) are used only for payment to non-default bank accounts. This suggests WHT data is not needed on the IVP, pending an end-to-end demo.
- Using the existing exemption start/end date fields for certificates of residency is disputed. The alternative is a separate data element.

Names come from an automatic transcript; please correct spellings.

## 1. Ordering-address extension and direct procurement

ODA extension to the purchasing org only is confirmed. A direct-buy from the domestic entity would need that entity set up as a separate SUPP vendor.

**Extension confirmed.** Nikki confirmed that ODA vendors are loaded with purchasing org extension only, not company code. Harshita agreed.

**New scenario raised (Sourabh).** Singapore may buy services directly from a domestic entity such as PwC Singapore, with invoice and payment to that entity. The ODA vendor has account group ODA (main supplier is SUPP) and no company code extension, so no FI postings or payment are possible against it. The question was whether its account group should change to SUPP.

**Team position (Ashish and others):**

- The scenario was never part of the global design discussions. Business must confirm it is real.
- Any approach must work globally, not just for Singapore.
- Agree the solution first, then data maintenance. Design or data changes are acceptable once the problem is clear.
- The delivering party may also be a third-party contractor engaged by the foreign vendor, not only its domestic branch.

**SUPP vs ODA:**

- A SUPP vendor goes through formal onboarding: Ariba long form, tax registration numbers, attachments, and usually a contract.
- An ODA vendor is created only to identify who delivers the service. It needs far less data (e.g. no tax registration number). Account group drives field status and mandatory roles in MDG.
- Counterpoint: migrated vendors were already onboarded per set ID in the old system. S/4 only avoids creating duplicates.
- Andy: if PwC Singapore were to become a direct supplier for 1500, it would have to be set up as a SUPP vendor. Ariba controls PO creation, so the Ariba team must confirm whether buyers can select a non-SUPP vendor.

## 2. Invoicing party (IVP)

IVPs route payment to a non-default bank account; the vendor itself stays the same. Whether accounting reads any IVP master data still needs an end-to-end demo.

**Why IVPs exist.** The main SUPP vendor holds all its bank accounts, one marked default. An IVP is created for each non-default account. Example from Nikki: PwC has accounts in India, UK and Singapore, so one is default and two IVPs are created. VIM cannot tell apart several bank accounts in the same country and takes the first, so IVPs make the choice explicit.

**ODA vs IVP in Ariba:**

|  | Ordering address (ODA) | Invoicing party (IVP) |
| --- | --- | --- |
| Stage | Upstream: PR/PO ("ship from" dropdown) | Downstream: invoice submission |
| Chosen by | Buyer/requestor | Supplier, via remit-to ID |
| Purpose | Shows on PO copy; identifies delivering party | Bank account for payment |
| Flows to S/4 | No, PO goes with main vendor | Yes, in VIM invoicing party field |

**IVP process:**

1. Onboarding team maps each extra bank account to an IVP.
2. A program emails the supplier the new IVP number and bank details.
3. The supplier creates a matching remit-to ID in the Ariba network.
4. At invoice submission the supplier selects the remit-to ID. In VIM, the supplier field holds the main vendor and the invoicing party field holds the IVP.

**Discussion points:**

- Andy's open question: when the invoice posts, which master data (WHT, account data) does accounting use, the main vendor's or the IVP's?
- Andy also questioned the extra build. A VIM business rule flagging a bank account that differs from the default might be simpler than full automation.
- An IVP is chosen per invoice, so one PO can have invoices from different IVPs.
- Kenya has an IVP relationship with an invoicing-party account group and the same external partner number as the main vendor. No transactions were found on it.
- For migrated vendors, search term 1 holds the SM ID and search term 2 the PeopleSoft ID.
- If confirmed, WHT data does not need to be maintained on the IVP.

## 3. Exemption fields, certificates and controls

No decision yet on where certificate-of-residency dates live. Both sides agreed certificates need a periodic control process, not a per-transaction check.

**Ashish's concern.** The exemption % and start/end date fields in vendor WHT data (LFBW) apply on top of the WHT rate. Example: a 10% rate with a 30% exemption gives 7%. India uses them for lower TDS deduction. Reusing them for DTA / certificate of residency in Singapore would make the same report mean different things by country. He proposes a separate data element.

**Andy's view.** The fields can serve several purposes. They are kept per vendor, per company code and per WHT type, and each company code is responsible for its own data. Example: one vendor active in 1500 and in India keeps COR dates under 1500 and TDS exemption data under the India company code. Gaps found by the report should trigger master data remediation.

**Other points:**

- LFBW's key includes WHT type. If a vendor has two types (e.g. software licence vs service, derived from commodity code), dates must be kept on both.
- WHT code is not maintained by MDG. If MDG maintained it, the system would not call BRF+.
- Two documents apply: the certificate of residency (has a validity period) and the non-PE declaration letter (only an issue date). The WHT calculation runs without these fields; they serve as evidence for applying the exemption.

**Certificates today.** Vendors attach documents to the Ariba long form at onboarding. The onboarding team checks values such as tax ID against them. It is unclear whether COR and non-PE letters are collected and stored; to be checked with Kaushal.

**Vendor report (ZMM vendor report).** Andy proposed formalising who uses this custom report and its inputs and outputs for SUPP, ODA and IVP vendors. Example: list all SUPP vendors in 1500 with WHT data to spot expiring certificates.

## Action items

The next call is tomorrow and should run longer, possibly starting an hour earlier. It will continue with the Singapore and Kenya ordering process.

| # | Action | Owner | Due |
| --- | --- | --- | --- |
| 1 | Get business confirmation that Singapore will procure directly from domestic delivering entities | Ashish / tax team | TBC |
| 2 | Confirm with the Ariba team whether buyers can select vendors with account group other than SUPP | Kranti | Next call |
| 3 | Arrange a UAT vendor with multiple ODAs and multiple IVPs (via onboarding, Saravana) | Kranti | Next call |
| 4 | Email Shrikant with the scenario background; invite him to demo a transaction end to end | Kranti | Next call |
| 5 | Find Kenya testing documentation on invoicing party / WHT | Sushil | TBC |
| 6 | Check how COR and non-PE letters are collected and stored at onboarding | TBC (with Kaushal) | TBC |
| 7 | Decide on a separate field vs existing LFBW exemption fields for COR dates | Ashish / Andy | TBC |
| 8 | Formalise ZMM vendor report usage, inputs and outputs per account group | Nikki | TBC |
| 9 | Schedule a longer call, possibly one hour earlier | TBC | Tomorrow |

## Open points

- [ ] Is direct procurement from a domestic delivering entity a real business scenario, and how should it work globally?
- [ ] When an invoice posts with an IVP, which vendor's master data drives the accounting entry and WHT?
- [ ] Is WHT data needed on IVPs at all?
- [ ] Should a VIM business rule replace part of the IVP build for bank account selection?
- [ ] Where should certificate-of-residency and non-PE dates be stored?
- [ ] What periodic control will monitor certificate validity, and who runs it?
