# Changelog

All notable changes to this fork of the Cross-Border Payment System are documented here.

This fork builds on [Mervespm/CrossBorderPaymentSystem](https://github.com/Mervespm/CrossBorderPaymentSystem), which was non-functional out of the box. The entries below cover the fixes applied to bring the application to a working, demoable state.

## [Unreleased / Fork Fixes]

### Fixed

- **Bank registration & login** — Bank signup and login were failing due to broken identity/session handling in the application layer. Repaired the registration flow and authentication pipeline so banks can be enrolled against the Fabric network and log in successfully.
- **Customer registration & login** — Customer signup and login suffered from the same class of failure as the bank flow, blocking customers from reaching their dashboard entirely. Fixed identity enrollment and session handling so customer accounts authenticate correctly.
- **Inter-customer transactions** — Transfers between two customers were failing outright rather than completing. Debugged and repaired the transaction submission logic so customer-to-customer transfers now process and settle correctly on the ledger.
- **Cross-border currency exchange** — Transactions between banks operating in different currencies (e.g. a USD bank and an Indian/INR bank) were erroring out instead of converting and settling. Fixed the currency conversion logic so cross-currency, cross-bank transfers complete with accurate converted amounts reflected on both sides of the transaction.

### Result

The application is now fully usable end-to-end: banks and customers can register, authenticate, and transact — including cross-currency, cross-bank transfers — with all activity correctly recorded and viewable on the Hyperledger Explorer.

---

## Upstream

For the original project history prior to this fork, see [Mervespm/CrossBorderPaymentSystem](https://github.com/Mervespm/CrossBorderPaymentSystem).
