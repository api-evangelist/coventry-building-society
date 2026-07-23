---
name: Confirm funds availability (CBPII)
description: Create a funds-confirmation consent, have the PSU authorise it, then check whether funds are available on a consented account through Coventry Building Society's Open Banking CBPII API.
api: openapi/obie-confirmation-funds-openapi.yaml
operations:
  - CreateFundsConfirmationConsents
  - GetFundsConfirmationConsentsConsentId
  - DeleteFundsConfirmationConsentsConsentId
  - CreateFundsConfirmations
---

# Confirm funds availability (CBPII)

Coventry Building Society implements the OBIE Read/Write Confirmation of Funds API (CBS
v2.0). A card-based payment instrument issuer (CBPII) confirms funds availability without
retrieving balances.

## Preconditions
- OAuth2 client-credentials token with scope `fundsconfirmations`.
- mTLS transport with a valid WAC/QWAC certificate.
- `x-idempotency-key` on funds-confirmation writes (24h retention).

## Steps
1. **Create the consent** — `CreateFundsConfirmationConsents`
   (POST `/funds-confirmation-consents`) referencing the debtor account and expiry.
   Returns a `ConsentId`.
2. **PSU authorisation (SCA)** — the PSU authorises the long-lived consent via
   authorization-code + OIDC.
3. **Confirm consent status** — `GetFundsConfirmationConsentsConsentId`; proceed when
   `Authorised`.
4. **Check funds** — `CreateFundsConfirmations` (POST `/funds-confirmations`) with the
   `ConsentId`, `Reference` and `InstructedAmount`. The response `FundsAvailableResult`
   returns a boolean `FundsAvailable`, never the balance.
5. **Revoke when done** — `DeleteFundsConfirmationConsentsConsentId` to withdraw the
   consent.

## Rules
- Only a boolean availability is returned — no balance or transaction data.
- Errors use the `OBErrorResponse1` envelope with `UK.OBIE.*` codes
  (see errors/coventry-building-society-problem-types.yml).
