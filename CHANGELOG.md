# Changelog

All notable changes to PostSharp are documented here.

## [2024.0.22] - 2026-03-18

### Bug Fixes
- Global named mutexes used for build coordination could cause `UnauthorizedAccessException` on shared build servers when different users run builds, due to missing ACL configuration (#7)
- PostSharp does not respect private NuGet feed configuration (#5)

### Enhancements
- Added memory throttling: the PostSharp MSBuild task now waits for sufficient available physical memory before starting compilation, controlled by the `POSTSHARP_REQUIRED_MEMORY` environment variable. This prevents out-of-memory conditions during parallel builds in memory-constrained environments such as Docker containers (#10)
- Decoupled Docker CPU access from build parallelism. The `POSTSHARP_MAX_PARALLELISM` environment variable now controls the maximum number of concurrent PostSharp compiler processes, allowing containers to use all available host CPUs while preventing memory exhaustion (#11)
