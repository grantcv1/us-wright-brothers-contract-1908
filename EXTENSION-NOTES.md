# Modeling a contract in Akoma Ntoso and LegalRuleML: what fit and what didn't

Notes from marking up the Wright Brothers' contract of 1908. It's one short contract, so this is a starting list, not a survey.

## Fit without strain

| Feature | Markup |
| --- | --- |
| Articles and numbered paragraphs | `article`, `paragraph`, `list`, `point` |
| Opening recital of the parties | `preamble`, with `party` and `as` |
| Blanks left empty | `fillIn` |
| Text missing from the source | `omissis` |
| Drafting note to the form | `authorialNote` |
| Specification incorporated into the contract | `attachment` holding a `doc` |
| Cross-references | `ref`, `rref` |

## Needed generic elements

| Feature | Generic markup | Why |
| --- | --- | --- |
| Document type | `doc name="contract"` | No `contract` document type |
| Filled-in blank | `inline name="filled"` | `fillIn` models an empty blank. Nothing marks a blank that has been completed |
| Typed text in a printed form | `inline name="typed"` | Printed form and entered text are different layers with no standard markup |
| The goods | `inline name="item"` | No element for the subject of a contract |
| Receipt stamp | `block name="receivedStamp"` | No element for an endorsement or stamp |
| Signature blocks | `container name="signatureBlock"` | `signature` exists, but nothing groups a signer with witnesses |
| Later approval | `container name="approval"` | No model for countersignature or approval |

## Stretched or missing in Akoma Ntoso

- **`signature` can't carry `as`.** The schema rejected it, so a signer's role can only sit on a `person` inside. The witnesses have a `refersTo` and no role.
- **`quantity` requires `refersTo` and has no unit.** Every value needed a concept reference. `normalized` is a plain string, so I put a bare number there, and no attribute says dollars or miles per hour.
- **No type for a date.** The delivery deadline is a `date` pointing to a concept. There's no standard way to say deadline, effective date or expiry.
- **Parties have no place of their own in the metadata.** `FRBRauthor` repeats, so I listed the Signal Corps and Wright Brothers as authors with their roles. That works, but it calls the parties authors, and the Wrights only signed the Army's form.
- **FRBR naming assumes a public jurisdiction.** I used `/akn/us/doc/contract/...` with an invented name. Private contracts have no registry or number to build on.
- **Life cycle events don't suit contracts.** This contract's deadline was extended, and delivery and acceptance followed. `eventRef` allows only `generation`, `amendment` and `repeal`, and the extensions aren't in this copy, so only execution is recorded.
- **Incorporation has no semantics.** The contract says the advertisement, specification and proposal "form a part of it." An attachment shows that they're attached, not that they bind or which prevails in a conflict.

## LegalRuleML: what it added

- **Who owes what:** `Obligation`, `Permission` and `Prohibition` with a `Bearer` cover delivery, payment, instruction, assignment and the congressional bar.
- **Remedies:** `PenaltyStatement` and `Reparation` connect the failure to deliver with the charge for buying elsewhere.
- **Link to the text:** `LegalReference` with `Association` ties every statement to its `eId`. This was the cleanest part.

## LegalRuleML: where it was awkward

- **Sliding price:** The scale needed one rule per speed. The real rule is arithmetic, and a table of facts hides that.
- **Rounding:** The scale gives whole miles per hour. They averaged 42.58 and were paid at 42, or 120 percent. The contract doesn't state that rule, so it's an interpretation, which belongs in `Alternatives` and isn't modeled.
- **"Should" and "must":** The specification mixes them, and "it is desirable." I marked all statements strict, which flattens that difference.
- **Deadlines:** Dates and durations are plain data arguments, not LegalRuleML temporal parameters.
- **Coverage:** The rules are a sample. They cover the articles except Article V, whose text is partly lost, and only paragraphs 4 to 7 and 13 of the specification.
- **Agents across standards:** `Agent` uses `sameAs` to point at Akoma Ntoso references. It works, but nothing defines that link.
- **Constitutive rules for conditions:** Article XI's condition on appropriation fits only loosely as a constitutive rule.

## Embedding

- `foreign` accepted LegalRuleML with no change. The Akoma Ntoso schema doesn't check what's inside.
- A separate `attachment` was the neatest home, since the rules aren't part of the contract's text. It does need its own `doc`, `meta` and a stand-in FRBR identity.
- The rules are hidden in the view. A reader sees only the contract and has to open the XML to see them.

## Questions for the extension

1. Should `contract` be a document type, or is `doc name="contract"` enough?
2. Should a filled-in blank be a form of `fillIn`?
3. Should `quantity` have a unit, and `date` a type?
4. How should parties appear in the metadata?
5. Which life cycle events should contracts have?
6. Should LegalRuleML stay in `foreign`, or should there be a defined binding?
