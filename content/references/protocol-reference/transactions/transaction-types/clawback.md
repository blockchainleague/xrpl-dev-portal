---
html: clawback.html
parent: transaction-types.html
blurb: Claw back tokens you've issued.
labels:
  - Tokens
status: not_enabled
---
# Clawback

[[Source]](https://github.com/XRPLF/rippled/blob/master/src/ripple/app/tx/impl/Clawback.cpp "Source")

{% include '_snippets/clawback-disclaimer.md' %}

Claw back tokens issued by your account.

Clawback is disabled by default. To use clawback, you must send an [AccountSet transaction][] to enable the **Allow Trust Line Clawback** setting. An issuer with any existing tokens cannot enable Clawback. You can only enable **Allow Trust Line Clawback** if you have a completely empty owner directory, meaning you must do so before you set up any trust lines, offers, escrows, payment channels, checks, or signer lists.  After you enable Clawback, it cannot reverted: the account permanently gains the ability to claw back issued assets on trust lines.

## Example Clawback JSON

```json
{
  "TransactionType": "Clawback",
  "Account": "rp6abvbTbjoce8ZDJkT6snvxTZSYMBCC9S",
  "Amount": {
      "currency": "FOO",
      "issuer": "rsA2LpzuawewSBQXkiju3YQTMzW13pAAdW",
      "value": "314.159"
    }
}
```

## Clawback Fields

{% include '_snippets/tx-fields-intro.md' %}

| Field              | JSON Type | [Internal Type][] | Description       |
|:-------------------|:----------|:------------------|:------------------|
| `Amount`           | [Currency Amount][]  | Amount |Indicates the amount being clawed back, as well as the counterparty from which the amount is being clawed back. The quantity to claw back, in the `value` sub-field, must not be zero. If this is more than the current balance, the transaction claws back the entire balance. The sub-field `issuer` within `Amount` represents the token holder's account ID, rather than the issuer's.|

The account executing this transaction must be the issuer of the asset being clawed back. Note that in the XRP Ledger, trust lines are bidirectional and, under some configurations, both sides can be seen as the *issuer* of an asset. In this specification, the term *issuer* is used to mean the side of the trust line that has an outstanding balance (that is, 'owes' the issued asset) that it wants to claw back.


## Error Cases

Besides errors that can occur for all transactions, {{currentpage.name}} transactions can result in the following [transaction result codes](transaction-results.html):

| Error Code | Description |
|:-----------|:------------|
| `temDISABLED` | Occurs if the [Clawback amendment](known-amendments.html#clawback) is not enabled. |
| `temBAD_AMOUNT` | Occurs if the holder's balance is 0. It is not an error if the amount exceeds the holder's balance; in that case, the maximum available balance is clawed back. Also occurs if the counterparty listed in `Amount` is the same as the `Account` issuing this transaction. |
| `tecNO-LINE` | Occurs there is no trust line with the counterparty or that trust line's balance is 0. |
| `tecNO-PERMISSION` | Occurs if you attempt to set `lsfAllowTrustlineClawback` while `lsfNoFreeze` is set. Also occurs, conversely, if you try to set `lsfNoFreeze` while `lsfAllowTrustLineClawback` is set. |

<!-- {# common link defs #} -->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
