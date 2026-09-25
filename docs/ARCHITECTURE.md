# Architecture — Reseller Commission Settlement

High-level reference for evaluating **reseller commission settlement** inside EVD / EVMS platforms. Educational only; vendor designs vary.

## Logical Components

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│ Sale / redeem   │────►│ Tariff & tier    │────►│ Accrual ledger  │
│ event bus       │     │ rate engine      │     │ pending/paid    │
└─────────────────┘     └────────┬─────────┘     └────────┬────────┘
                                 │                        │
                                 ▼                        ▼
                        ┌──────────────────┐     ┌─────────────────┐
                        │ Settlement cycle │     │ Payout / netting│
                        │ + clawback rules │     │ bank / MoMo     │
                        └──────────────────┘     └─────────────────┘
```

## Suggested Accrual States

| State | Meaning |
|-------|---------|
| `pending` | Calculated, not yet in a closed cycle |
| `held` | Risk or redeem hold before approval |
| `approved` | Included in closed settlement cycle |
| `paid` | Included in a successful payout file |
| `clawed_back` | Reversed after void/fraud/policy |

## Audit Events Worth Keeping

- Qualifying sale / redeem / void  
- Tariff version applied  
- Accrual create / hold / approve  
- Cycle close checksum  
- Payout file generation and bank ack  

Live product reading: [EVMS page](https://evdsystem.com/electronic-voucher-management-system/), [evdsystem.com](https://evdsystem.com/).
