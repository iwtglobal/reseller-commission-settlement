# Reseller Commission Settlement

An educational guide to **reseller commission settlement** in electronic voucher distribution (EVD): how multi-tier partners earn, accrue, and get paid on voucher and airtime sales. Written for fintech, telecom, and payment teams evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Reseller Commission Settlement?

**Reseller commission settlement** is the process that calculates, accrues, and pays (or nets) earnings owed to distributors, sub-dealers, and agents for selling electronic vouchers, airtime, gift cards, or related digital value. Settlement closes the commercial loop between issuer margin, wholesale discount, and retail sell price.

In EVD networks, commission may be upfront discount on float purchase, backend rebate on redeemed volume, or hybrid. Weak settlement design causes partner disputes, opaque overrides, and finance breaks between sales reports and bank payouts.

### Why commission settlement matters

- **Partner trust** — transparent accruals reduce churn in agent networks  
- **Finance close** — issuer P&L needs clean commission expense cuts  
- **Multi-tier fairness** — parent overrides must not silently steal child earnings  
- **Audit** — regulators and auditors expect reconstructible payout trails  

Commission settlement is not a spreadsheet afterthought; it is a core EVD commercial control.

---

## Architecture Overview: Commission Pipeline

```
Sale / redeem event (POS / API / USSD)
        │
        ▼
Tariff & tier engine (who earns what)
        │
        ▼
Accrual ledger (pending commission)
        │
        ▼
Settlement cycle (daily / weekly / monthly)
        │
        ├──► Net against float / inventory debt
        └──► Payout file → bank / mobile money
```

### Core layers

| Layer | Responsibility |
|-------|----------------|
| **Event capture** | Sale, redeem, reverse, and void with partner IDs |
| **Tariff engine** | Maps product, channel, and tier to commission rates |
| **Accrual ledger** | Pending, approved, paid, and clawed-back amounts |
| **Settlement runner** | Closes cycles, nets debt, produces payout files |
| **Partner portal** | Statements, exceptions, and dispute workflows |

EVMS platforms keep commission events append-only so partners and finance share one statement of record.

---

## How Reseller Commission Settlement Works

### 1. Capture qualifying events

Each sale (and sometimes redeem) records the selling node, parent chain, product SKU, and face value. Reversals create offsetting events, not silent deletes.

### 2. Apply tariff and tier rules

Rates may be percent of face, fixed per txn, or margin share. Multi-tier trees assign child commission and parent override from the same event.

### 3. Accrue pending commission

Amounts post to a pending ledger keyed by partner and settlement cycle. Holds apply for fraud or incomplete redeem where policy requires.

### 4. Run the settlement cycle

At cycle close, pending moves to approved (minus clawbacks). Netting may reduce float top-up debt before cash payout.

### 5. Pay and reconcile

Payout files go to bank or mobile money. Partners reconcile statements; exceptions open cases with the original event IDs.

---

## Patterns and Use Cases

1. **Upfront wholesale discount** — Reseller buys float cheaper; “commission” is embedded in purchase price.  
2. **Backend rebate** — Accrue on sold or redeemed volume; pay on a cycle.  
3. **Hybrid EVD** — Discount on stock plus override on child sales.  
4. **Airtime eTopup** — Commission on direct top-up API volume by channel.  
5. **Gift card reseller chains** — Activation-triggered commission with brand-specific tariffs.

Platforms such as EVD System / MoboGage implement reseller commission settlement alongside multi-tier distribution and inventory so partner economics stay aligned with voucher lifecycle events.

---

## Implementation Considerations

- **Event-sourced accruals** — never overwrite; reverse with new events  
- **Tier tree versioning** — rate changes must not rewrite historical accruals  
- **Clawback policy** — define windows for void, fraud, and unredeemed stock  
- **Netting rules** — document when commission offsets float debt vs. cash pay  
- **Currency and tax** — withholdings and multi-currency statements need clear rules  
- **Statement grain** — partners need event-level and cycle-level views  

Choosing a settlement model should prioritize reconstructible statements over opaque “net paid” summaries.

---

## FAQ

**Is wholesale discount the same as commission?**  
Economically related, but discount is at purchase; backend commission accrues on later events and settles on a cycle.

**When does commission become payable?**  
Per product policy — on sale, on redeem, or after a hold period. Document the trigger clearly.

**What is an override?**  
Parent-tier earnings on child volume, calculated from the same sale event under tariff rules.

**How do voids affect settlement?**  
They should create clawback events in the accrual ledger, ideally before payout; post-payout clawbacks need collection workflows.

**Can agents see event-level detail?**  
Best practice: yes, via partner portal statements keyed to sale/redeem IDs.

**How does this relate to MoboGage / EVD System?**  
EVD System is MoboGage’s electronic voucher distribution and management platform family; reseller commission settlement is how multi-tier EVD partners get paid on voucher and related sales. See the [electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/) overview when evaluating product fit.

---

## Further Reading / Related Industry Resources

- [Electronic voucher management system](https://evdsystem.com/electronic-voucher-management-system/) — EVMS product context  
- [EVD System home](https://evdsystem.com/) — platform overview for digital value distribution  

See also [docs/ARCHITECTURE.md](./docs/ARCHITECTURE.md) for a component view.

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not a substitute for vendor due diligence or regulatory advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
