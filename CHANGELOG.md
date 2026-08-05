# Changelog

All notable changes to PostSharp are documented here.

## [2024.0.25] - 2026-08-05

### Enhancements
- Removed the obsolete watermark task, which could abort a compilation with a PS0099 unhandled `NullReferenceException` when a user license entry was null. Woven assemblies no longer carry the `EnhancedByPostSharp` manifest resource, PS0129 (*This module has already been processed by PostSharp*) is no longer emitted, and `WatermarkTask`, `TaskNames.Watermark` and `License.RequiresWatermark` have been removed from the compile-time SDK (#75)

### Bug Fixes
- Fixed crashes of the Customer Experience Improvement Program uploader when the upload queue was empty, when a queued file was locked, or when BITS refused the transfer job. Orphan BITS jobs left behind by earlier failures are now cleaned up, a job that makes no progress for three days is abandoned, and the pending-report queue is capped at 50 files and 30 days (#77)

## [2024.0.24] - 2026-07-24

### Bug Fixes
- Fixed concurrency defects in the wait primitive behind `BackgroundTaskScheduler`, which could hang or crash the disposal of a caching back-end: a lost wakeup caused by a missing memory fence, an asynchronous continuation scheduled twice, and a wait operation that could be woken spuriously by an unrelated wait on the same thread (#70)
- Fixed an `IndexOutOfRangeException` thrown when reading a module whose `MethodDef` table contains exactly 65535 rows (#71)
- Fixed the detection of `TargetFrameworkAttribute` and `ReferenceAssemblyAttribute` in the assemblies that define these attributes themselves, such as mscorlib, System.Runtime, netstandard and the reference assemblies (#72)
- A method whose `Param` table disagrees with its signature is now reported as invalid metadata instead of failing the build with an `IndexOutOfRangeException` (#73)
- The `Flags` columns of the metadata tables are now read as unsigned values, so that the attributes of a literal field, or of a method that carries declarative security, no longer have their upper 16 bits set (#74)
- Fixed a PS0264 assembly load failure that occurred when a project referenced a higher version of a package that the PostSharp compiler also depends on, and the build reused the cached `deps.json` (#76)

### Other Changes
- Updated the EULA to v2026-Q3 (#68)

## [2024.0.23] - 2026-06-27

Primarily a security and privacy hardening release, closing the June 2026 security and privacy review (#56).

### Security & Privacy

Transport security:
- Upload CEIP/telemetry over HTTPS instead of plaintext HTTP (#48)
- Warn when a license server is configured over `http://`, which sends user and machine names in cleartext (#63)

Local-machine hardening on shared and multi-user machines:
- Default the cache, binary and dependency directories to per-user locations to prevent DLL planting (#45)
- Restrict the .NET Framework build pipe server to clients of the same user and elevation level (#46)
- Scope the Windows CPU-throttle semaphore to the current user to prevent a local build denial of service (#50)
- Relocate the compiler-host exception dump to a per-user directory so that other local users cannot read it (#51)
- Disable external XML entity resolution (XXE) when parsing the Learning Hub content feed (#47)

Data minimization:
- Minimize uploaded exception reports: redact secrets, omit the exception message and data, and redact user assembly identities (#53)
- Telemetry opt-out now stops queued uploads and purges the upload queue (#54)
- Removed the per-usage license telemetry that uploaded reversibly hashed type names (#55)
- Rotate the telemetry device identifier on the first Monday of each month to limit cross-session correlation (#62)
- Removed the newsletter subscription offer and all email-address collection from the license registration UI (#65)
- Removed the Areas of Interest selection and the PostSharp Learning Hub tool window from the product (#66)

Dependencies:
- Updated log4net to 3.3.1 on modern target frameworks to address CVE-2026-40021 (#61)

### Bug Fixes
- Deterministic builds emitting a Windows PDB are now reproducible for woven types with very long fully qualified names (#35)
- Windows PDBs no longer get garbage, run-dependent module names for woven types with long fully qualified names (#36)

### Enhancements
- Added the `PostSharpAllowPipeServerWhenUnattended` MSBuild property to use the pipe server in unattended builds (#58)

## [2024.0.22] - 2026-03-18

### Bug Fixes
- Global named mutexes used for build coordination could cause `UnauthorizedAccessException` on shared build servers when different users run builds, due to missing ACL configuration (#7)
- PostSharp dependency restore ignores NuGet.config (#5)

### Enhancements
- Added memory throttling: the PostSharp MSBuild task now waits for sufficient available physical memory before starting compilation, controlled by the `POSTSHARP_REQUIRED_MEMORY` environment variable. This prevents out-of-memory conditions during parallel builds in memory-constrained environments such as Docker containers (#10)
- Decoupled Docker CPU access from build parallelism. The `POSTSHARP_MAX_PARALLELISM` environment variable now controls the maximum number of concurrent PostSharp compiler processes, allowing containers to use all available host CPUs while preventing memory exhaustion (#11)
