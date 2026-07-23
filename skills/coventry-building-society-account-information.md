---
name: Retrieve consented account data (AIS)
description: Set up an account-access consent, have the PSU authorise it via SCA, then read the customer's accounts, balances and transactions from Coventry Building Society's Open Banking AIS API.
api: openapi/obie-account-info-openapi.yaml
operations:
  - CreateAccountAccessConsents
  - GetAccountAccessConsentsConsentId
  - GetAccounts
  - GetAccountsAccountId
  - GetAccountsAccountIdBalances
  - GetAccountsAccountIdTransactions
---

# Retrieve consented account data (AIS)

Coventry Building Society implements the OBIE Read/Write Account & Transaction
Information API (CBS v2.0). All access is consent-scoped and FAPI-secured. You must be a
registered TPP with a valid Open Banking WAC or eIDAS QWAC certificate and mutual-TLS.

## Preconditions
- OAuth2 client-credentials token with scope `accounts` (for consent creation).
- mTLS transport established with a valid WAC/QWAC certificate.
- Send `x-fapi-interaction-id` (UUID) on every request for tracing.

## Steps
1. **Create the consent** — `CreateAccountAccessConsents` (POST `/account-access-consents`)
   with the requested `Permissions[]`, `ExpirationDateTime`, and transaction date range.
   Returns a `ConsentId` in status `AwaitingAuthorisation`.
2. **PSU authorisation (SCA)** — redirect the PSU through the authorization-code + OIDC
   flow so they complete strong customer authentication. The resulting access token is
   bound to the `ConsentId`.
3. **Confirm consent status** — `GetAccountAccessConsentsConsentId`
   (GET `/account-access-consents/{ConsentId}`); proceed only when status is `Authorised`.
4. **List accounts** — `GetAccounts` (GET `/accounts`) to obtain each `AccountId`.
5. **Read detail** — `GetAccountsAccountId`, `GetAccountsAccountIdBalances`,
   `GetAccountsAccountIdTransactions` for balances and transactions. Use the `Links.Next`
   field to page; filter transactions with `fromBookingDateTime` / `toBookingDateTime`.

## Rules
- Errors use the `OBErrorResponse1` envelope with namespaced `UK.OBIE.*` codes
  (see errors/coventry-building-society-problem-types.yml).
- Respect HTTP 429 polling limits; back off per the OBIE Operational Guidelines.
- Never request permissions beyond what the use case needs — consent is least-privilege.
