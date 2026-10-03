# Changelog

All notable changes to PostSharp are documented here.

## [2027.0.2-preview] - 2026-10-03

Second preview of PostSharp 2027.0, based on 2027.0.1-preview.

### New
- Advice parameter binding is now a publicly supported feature, after years of internal use by the Pattern Libraries (#168)
- Added support for runtime-generated async methods (`<Features>runtime-async=on</Features>` on .NET 11) (#187)
- A repository can opt out of telemetry by setting `TelemetryEnabled` to `False` in the `postsharp.config` file at its root (#188)

### Enhancements
- `[SelfPointcut]` and `Master` can be omitted in a declarative aspect when there is only one way to group the advices (#192)

### Fixes
- Fixed an internal compiler exception, or an advice silently disabled, when an advice method has an invalid parameter binding (#161)
- Fixed invalid IL, or an internal compiler exception, when an advice with bound parameters does not match its target (#162)
- The `PostSharp.Tool` package is now published (#171)
- Warnings about the repository configuration are now reported as PS0284 build warnings (#182)

## [2027.0.1-preview] - 2026-09-29

First preview of PostSharp 2027.0, based on 2026.0.18.

### Breaking Changes
- `net6.0`, `net8.0` and `net9.0` are no longer supported target frameworks, and the packages no longer ship assets for them. The minimum SDK at build time is the .NET 10 SDK (#87)
- The PostSharp Options application is removed and replaced by a web-based user interface shared with Metalama, opened from toast notifications. `PostSharp.Settings.Common`, `PostSharp.Settings.AbstractUI` and `PostSharp.Settings.PostSharpIL` are no longer published (#114)
- Interactive builds on Linux and macOS now require a licence (#117)
- Usage telemetry is switched on at the first build unless the user opts out (#118)
- Add-ins compile against the reference assemblies of the new `PostSharp.Sdk` package (#142)
- Builds on Windows build agents now use the pipe server, which keeps running after the build. `PostSharpAllowPipeServerWhenUnattended` is obsolete (#145)

### New
- Added the `postsharp` command line tool, which registers licence keys and edits settings (#119)
- Added `postsharp shutdown`, which stops the PostSharp processes that keep running after a build (#124)
- Licence keys signed with Elliptic Curve DSA are now supported (#170)

### Bug Fixes
- Fixed an intermittent build failure with `ObjectDisposedException: Safe handle has been closed` while assembly identities are read concurrently (#115)
- Fixed a build failure with `PS0242` in a linked git worktree when the licence key is limited to a namespace (#116)
- Fixed, on Linux and macOS, a package whose version contains an upper-case letter being left out of the dependencies of the compiler (#154)
- Fixed a hang of the Redis caching backend in `Clear`, `InvalidateDependency` or `CleanUpAsync` (#159)
- Fixed a `NullReferenceException` in `MultiplexerBackend` when a child logging backend creates no transaction (#160)

### Known Issues
- The `PostSharp.Tool` package was not uploaded to nuget.org with this release by mistake. It will be published with the next release (#171)
