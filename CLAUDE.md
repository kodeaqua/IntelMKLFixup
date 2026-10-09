# IntelMKLFixup

Lilu plugin (macOS kext, x86_64) that patches `_mkl_serv_intel_cpu_true` in Intel MKL
in memory so MKL-linked apps work on AMD Hackintoshes.

## Layout
- `IntelMKLFixup/IntelMKLFixup.cpp` – the whole plugin: byte pattern/mask, `_cs_validate_page`
  (Big Sur+) / `_cs_validate_range` (High Sierra) routing, `PluginConfiguration`.
- `IntelMKLFixup/Info.plist` – kext metadata and Lilu/KPI dependencies.
- `.github/workflows/main.yml` – CI build (Debug + Release) via MacKernelSDK and Lilu bootstrap.

## Build
Requires Xcode, `MacKernelSDK` and `Lilu.kext` in the project root (CI bootstrap scripts fetch them):

    xcodebuild -jobs 1 -configuration Debug
    xcodebuild -jobs 1 -configuration Release

Version comes from `MODULE_VERSION` in the Xcode project build settings.

## Conventions
- Boot args: `-imklfxoff`, `-imklfxdbg`, `-imklfxbeta`.
- `wrapCsValidate*` runs on every validated page: keep the no-match path cheap (no path lookups, no logging).
- Max supported kernel is set in `PluginConfiguration` (`KernelVersion::Tahoe`); bump when Lilu adds newer constants.
- Commits: conventional prefixes (`feat:`, `fix:`, `perf:`, `chore:`), English messages.
