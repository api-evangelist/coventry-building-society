# Coventry Building Society (coventry-building-society)

Coventry Building Society is the United Kingdom's second-largest building society, a member-owned mutual founded in 1884 and headquartered in Coventry, offering savings and residential mortgages and, since January 2025, owner of The Co-operative Bank. As an FCA-authorised, PRA-regulated ASPSP it participates in UK Open Banking under PSD2 and the OBIE standards, publishing CBS v2.0 Account Information, Payment Initiation, and Confirmation of Funds APIs plus a public FCA Service Metrics open-data endpoint through its developer portal.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/coventry-building-society/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/coventry-building-society/refs/heads/main/apis.yml)

## Tags

- Financial Services
- Banking
- Building Society
- Open Banking
- PSD2
- OBIE
- United Kingdom
- Payments
- Account Information
- Confirmation of Funds
- Fintech

## Timestamps

- **Created:** 2026-07-23
- **Modified:** 2026-07-23

## APIs

### Coventry Building Society Open Data FCA Service Metrics API

Public, unauthenticated OBIE Open Data endpoint publishing FCA service metrics for personal current accounts (CBS v1.0). The live JSON host was confirmed responding at the documented base URL, though the `/pca` endpoint returned an HTTP 500 JSON error rather than a 200 at review time.

- **Human URL:** [https://developer.coventrybuildingsociety.co.uk/](https://developer.coventrybuildingsociety.co.uk/)
- **Base URL:** `https://connect.coventrybuildingsociety.co.uk/pd/digital/open-banking/v1.0`

#### Properties

- [Documentation](https://developer.coventrybuildingsociety.co.uk/)
- [API Reference](https://openbanking.atlassian.net/wiki/spaces/DZ/pages/195068329/FCA+Service+Metrics+API+Specification+-+v1.0.0)
- [OpenAPI](openapi/obie-opendata-swagger.json) — shared OBIE Open Data standard (v1.3), not a Coventry proprietary contract

### Coventry Building Society Account & Transaction Information API (AIS)

OBIE Read/Write AIS API (CBS v2.0) giving consented third parties read access to account, balance, transaction, standing order, direct debit, beneficiary, and product data. FAPI-secured (OAuth2/OIDC, PSD2 SCA, mTLS); authenticated, onboarding required.

- **Human URL:** [https://developer.coventrybuildingsociety.co.uk/](https://developer.coventrybuildingsociety.co.uk/)
- **Base URL:** `https://connect.coventrybuildingsociety.co.uk/pd/digital/open-banking/v2.0/aisp`

#### Properties

- [Documentation](https://developer.coventrybuildingsociety.co.uk/)
- [API Reference](https://openbankinguk.github.io/read-write-api-site3/)
- [OpenAPI](openapi/obie-account-info-openapi.yaml) — shared OBIE Read/Write standard, not a Coventry proprietary contract

### Coventry Building Society Payment Initiation API (PIS)

OBIE Read/Write PIS API (CBS v2.0) enabling consented third-party initiation of domestic single, scheduled, standing order, and file payments. FAPI-secured (OAuth2/OIDC, PSD2 SCA, mTLS); authenticated, onboarding required.

- **Human URL:** [https://developer.coventrybuildingsociety.co.uk/](https://developer.coventrybuildingsociety.co.uk/)
- **Base URL:** `https://connect.coventrybuildingsociety.co.uk/pd/digital/open-banking/v2.0/pisp`

#### Properties

- [Documentation](https://developer.coventrybuildingsociety.co.uk/)
- [API Reference](https://openbankinguk.github.io/read-write-api-site3/)
- [OpenAPI](openapi/obie-payment-initiation-openapi.yaml) — shared OBIE Read/Write standard, not a Coventry proprietary contract

### Coventry Building Society Confirmation of Funds API (CBPII)

OBIE Read/Write CBPII API (CBS v2.0) letting a consented card-based payment instrument issuer confirm whether funds are available on an account. FAPI-secured (OAuth2/OIDC, PSD2 SCA, mTLS); authenticated, onboarding required.

- **Human URL:** [https://developer.coventrybuildingsociety.co.uk/](https://developer.coventrybuildingsociety.co.uk/)
- **Base URL:** `https://connect.coventrybuildingsociety.co.uk/pd/digital/open-banking/v2.0/cbpii`

#### Properties

- [Documentation](https://developer.coventrybuildingsociety.co.uk/)
- [API Reference](https://openbankinguk.github.io/read-write-api-site3/)
- [OpenAPI](openapi/obie-confirmation-funds-openapi.yaml) — shared OBIE Read/Write standard, not a Coventry proprietary contract

## Common Properties

- [Website](https://www.coventrybuildingsociety.co.uk/)
- [Developer Portal](https://developer.coventrybuildingsociety.co.uk/)
- [Open Banking](https://www.coventrybuildingsociety.co.uk/member/help/savings/open-banking.html)
- [LinkedIn](https://www.linkedin.com/company/coventry-building-society)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
