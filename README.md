# Minutes of Meeting – Withholding Tax: Ordering Address & Service Dates
Nov 9, 2026 · @Ram Krupa
## Meeting details
The call clarified how ordering addresses drive withholding tax (WHT) for Singapore, and agreed to run daily requirement calls from Wednesday to close open points this week.
| Item | Detail |
| --- | --- |
| Date | Monday, 9 November 2026 |
| Topic | WHT requirements: ordering address, vendor extension, service start/end dates |
| Participants (as heard) | Sourabh, Harshita, Kartik, Vijay, Ashish, Rishish, Amos (dropped early) |
| Mentioned, not present | Kiki (to schedule follow-up calls), MDM team |
Names come from an automatic transcript; please correct spellings and add anyone missing.
## 1. Ordering address: Singapore vs Kenya
The ordering address + BRF+ setup for WHT applies only to Singapore; Kenya uses ordering addresses in the standard way.
**Kenya.** An ordering address shows where goods ship from or services are rendered, e.g. a vendor headquartered in Belgium supplying from another country. Harshita recalled Kenya vendors with ordering addresses in several countries, tested in UAT, and suggested bringing in the MDM team who loaded the data.
**Singapore.** Who delivers the service on the ground affects WHT, not only who invoices. Example: PwC UK is the main (invoicing) vendor, but PwC Singapore delivers the service. The delivering party may be the vendor's Singapore entity or an outsourced third party in Singapore or elsewhere.
How it works today (Sourabh):
1. MDM links the domestic vendor to the foreign main vendor as a partner function (ordering address).
2. In Ariba, selecting the main vendor shows its linked ordering addresses; the buyer picks the delivering party on the PO.
3. The PO goes to S/4 with the main vendor; the ordering address prints on the PO copy. An earlier issue where the selection did not flow to S/4 is now fixed.
4. ABAP code in S/4 calls BRF+, which checks whether the partner-function vendor differs from the main vendor. If so, it reads that vendor's country to determine WHT.
Intent: a Singapore party delivering on behalf of a foreign vendor should not attract WHT just because the purchase is from abroad. Currently all Singapore scenarios still go to exception.
## 2. Extending the ordering-address vendor to the Singapore company code
Not concluded: extension to the company code is needed only if Singapore will procure directly from the delivering entity.
- **Sourabh's view:** if Singapore plans to buy directly from PwC Singapore, that vendor must be extended to the Singapore company code with WHT details maintained. Otherwise it is not needed.
- **MDM setup:** an ordering address is only an address, not a vendor tied to a legal entity you can procure from. It gets its own vendor number (e.g. 789 vs the main vendor's 1234) but is not a SUPP account type; a SUPP vendor would have a different number. Domestic vendors with several addresses in Singapore also use ordering addresses, since multiple addresses can't be held on one vendor record.
- **Harshita:** in earlier workshops the team said ordering addresses also need extension for the country seen on the PO, whether or not it integrates to S/4.
- **Purchasing org vs company code:** the group separated extension to the purchasing organisation (purchasing view) from the company code (financial view). A system walkthrough of the business partner showed the supplier role extended to purchasing org 1500. The first BP opened was a person-type record, not a good example.
- Account group drives which fields are mandatory or optional in the MDG change request.
## 3. Service start/end date for WHT exemption
A new service start/end date attribute is needed; the existing PO start/end date will not serve this purpose.
- **Why it is manual today:** FI has no service start/end date. WHT is calculated on the invoice date, so there is no way to record service dates or use them to decide exemption.
- **Definition:** the period when the service is actually delivered, not the PO date or invoice date. Example: a PO raised today for services running until end of next year.
- **Singapore complexity:** one invoice can cover a full year (e.g. a 12-month software subscription), but WHT must be allocated across each of the 12 months, not against the single invoice.
- **Conclusion (Vijay, Kartik):** the PO start/end date does not meet this need. A new field is required, linked to the PO.
- **Current process:** service dates are tracked manually in an Excel file, collected directly from purchase requestors. Harshita's earlier understanding that R2R maintains it was corrected. It may be confirmed around receipt, but there is no receipting process for these services.
## Way forward and action items
Daily requirement calls start Wednesday Nov 11, 2026 (Tuesday 10 Nov is a Bangalore public holiday), aiming to close requirements this week. Requirements come first; feasibility follows.
| # | Action | Owner | Due |
| --- | --- | --- | --- |
| 1 | Schedule daily recurring calls from Wednesday | Kiki | Nov 11, 2026 |
| 2 | Lead requirement capture for all WHT dimensions (ordering address, service dates, certificates, etc.) | Ashish | This week |
| 3 | Walk Harshita through the current manual service-date process | Kartik | Next call |
| 4 | Bring the MDM team in to confirm how ordering addresses are loaded and linked | TBC | Next call |
| 5 | Test whether ordering-address vendors need company code / purchasing org extension | Harshita / Sourabh | TBC |
| 6 | Share open points from the other side so they can be added to the list | Harshita | Before next call |
| 7 | Circulate minutes of this call | TBC | Nov 11, 2026 |
| 8 | Set up a separate parallel call on cost assignment determination | TBC | TBC |
| 9 | Follow up with Amos, who dropped off while driving | TBC | TBC |
**Parallel track:** for the gross scenario, cost assignment should be picked from the BP rather than the default. Seen as a low-hanging fruit, to run in a separate call rather than queue behind the WHT topics.
## Open points
- [ ] Is extension of the ordering-address vendor to the Singapore company code required, or only to the purchasing org?
- [ ] Will Singapore ever procure directly from the delivering entity (e.g. PwC Singapore) as a SUPP vendor?
- [ ] Confirm with MDM whether Kenya ordering addresses span multiple countries, and how they are set up.
- [ ] Agree the definition and capture point of the service start/end date, and how it links to the PO.
- [ ] Design monthly WHT allocation for a single invoice covering a multi-month service period.
- [ ] Clarify the remaining WHT areas, including certificates.
