---
name: Initiate a domestic payment (PIS)
description: Create a domestic payment consent, have the PSU authorise it with SCA, then execute an idempotent, JWS-signed domestic payment through Coventry Building Society's Open Banking PIS API.
api: openapi/obie-payment-initiation-openapi.yaml
operations:
  - CreateDomesticPaymentConsents
  - GetDomesticPaymentConsentsConsentId
  - GetDomesticPaymentConsentsConsentIdFundsConfirmation
  - CreateDomesticPayments
  - GetDomesticPaymentsDomesticPaymentId
---

# Initiate a domestic payment (PIS)

Coventry Building Society implements the OBIE Read/Write Payment Initiation API (CBS
v2.0). Payment creation is idempotent and requires a detached JWS signature.

## Preconditions
- OAuth2 client-credentials token with scope `payments` (for consent creation).
- mTLS transport with a valid WAC/QWAC certificate.
- `x-jws-signature` (detached JWS, RFC 7515) on payment-consent and payment writes.
- `x-idempotency-key` (≤40 chars, unique per instruction) on writes — replays within 24h
  return the original resource, never a duplicate payment.

## Steps
1. **Create the payment consent** — `CreateDomesticPaymentConsents`
   (POST `/domestic-payment-consents`) with the `Initiation` (creditor account, instructed
   amount, reference). Returns a `ConsentId` in `AwaitingAuthorisation`.
2. **PSU authorisation (SCA)** — redirect the PSU through authorization-code + OIDC to
   authorise the specific payment. Token is bound to the `ConsentId`.
3. **(Optional) Confirm funds** — `GetDomesticPaymentConsentsConsentIdFundsConfirmation`
   to check funds availability before execution.
4. **Confirm consent** — `GetDomesticPaymentConsentsConsentId`; proceed only when
   `Authorised`.
5. **Execute the payment** — `CreateDomesticPayments` (POST `/domestic-payments`) with the
   `ConsentId` and an `Initiation` that exactly matches the consent. Supply
   `x-idempotency-key` and `x-jws-signature`.
6. **Track status** — `GetDomesticPaymentsDomesticPaymentId` to poll the payment status.

## Rules
- The `Initiation` block in the payment MUST match the authorised consent exactly, or the
  ASPSP returns `UK.OBIE.Resource.ConsentMismatch`.
- Missing/invalid signature → `UK.OBIE.Signature.Missing` / `UK.OBIE.Signature.Invalid`.
- Errors use the `OBErrorResponse1` envelope (see errors/coventry-building-society-problem-types.yml).
