# Irisin 5.1.8 - Compatibility Mode Restoration

An **unofficial** restoration of Compatibility Mode (rootless-to-RootHide package conversion) on top of upstream [Lakr233/Irisin](https://github.com/Lakr233/Irisin) v5.1.8.

Not affiliated with, or endorsed by, OwnGoal Studio or the upstream Irisin project. Irisin is MIT-licensed, Copyright (c) 2026 OwnGoal Studio; the original license text is retained in [LICENSE](LICENSE).

## What this is

Upstream Irisin v5.1.8 removed Compatibility Mode, which converted rootless (`iphoneos-arm64`) packages for RootHide jailbreaks inside the app, and hid packages whose architecture does not match the device. This repository contains a single patch that restores that functionality on top of the v5.1.8 source tree:

- Restores Compatibility Mode and the rootless-to-RootHide package conversion path (reverts upstream removal commits `875c0d0a` and `b2b0051d`).
- Restores visibility of rootless (`iphoneos-arm64`) packages on RootHide (`arm64e`) devices by adjusting architecture filtering.
- Restores the conversion test suites (`RootlessToRoothideTests`, `RootlessToRoothideTextTests`); they build and pass against the exact tree the tested package was built from.
- Resolves a `BootstrapArchitecture` definition conflict between modules, keeping its canonical definition in `IrisinProtocol`.

This is Irisin 5.1.8 with the removed compatibility feature restored, not a downgrade to 4.7.5.

## How to apply

```sh
git clone https://github.com/Lakr233/Irisin.git
cd Irisin
git checkout v5.1.8
git apply /path/to/0001-Restore-Compatibility-Mode-on-5.1.8-v2.patch
```

The patch is [`0001-Restore-Compatibility-Mode-on-5.1.8-v2.patch`](0001-Restore-Compatibility-Mode-on-5.1.8-v2.patch) in this repository (SHA-256: `2e3c2699ed92ff26362e0f8d7cd1a1fb8c2bf03e600d2fa98cf9bd37c34851e9`).

## Verification status

- Host test suites pass on the exact patched tree: IrisinKit 207 tests / 17 suites, AptRepository 176 tests / 22 suites, including the restored conversion suites.
- The patched tree compiles for iOS (app, daemon, helper, CLI); the resulting packages passed the repo's own `verify-deb.sh` checks and the iOS 15.0 minimum audit.
- One on-device trial succeeded: the modified app runs and a rootless package was converted and installed on a RootHide device (iPhone SE 2020, iOS 16.4). This does not establish compatibility with every package.

## License

Upstream Irisin is distributed under the MIT License (see [LICENSE](LICENSE)). The modifications in this repository are released under the same MIT License. Third-party components fetched by the build keep their own licenses; if you distribute compiled packages, review and preserve their notices as required.
