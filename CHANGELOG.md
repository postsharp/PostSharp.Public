# Changelog

All notable changes to PostSharp are documented here.

## [2027.0.7-preview] - 2026-10-27

Seventh preview of PostSharp 2027.0, based on 2027.0.6-preview. The packages of 2027.0.6-preview were never published to nuget.org, so this is the first build that ships its changes.

### Highlights
- `postsharp introspect`: query how aspects were applied to a project or a solution from the command line, in JSON for AI agents (#332)

### New
- `postsharp introspect show`, `list`, `schema`, `metrics` and `batch` in the `PostSharp.Tool` package (#332)
- Aspect testing: `.t.pssym` snapshots of the weaving symbol file with `@CompareWeavingSymbols` (#333)
- SharpCrafters.Backstage: the `SharpCrafters.Aspects.Introspection` and `SharpCrafters.Aspects.Introspection.Query` packages that the commands run on (SharpCrafters.Backstage#38, #39, #40, #41)

## [2027.0.6-preview] - 2026-10-22

Sixth preview of PostSharp 2027.0, based on 2027.0.5-preview.

### Highlights
- Dependency injection in aspects (#318), first used by `[Cache]` and `[InvalidateCache]` (#323)
- Background compilation server for .NET projects under the .NET SDK, previously available only for .NET Framework (#322)

### Breaking Changes
- ⚠️ Advice grouping: the `Group` property, `Master` and `[SelfPointcut]` obsolete, and `[SelfPointcut]` advices of different kinds form one group (#324)
- ⚠️ .NET projects are compiled in a pipe server by default under the .NET SDK (#322)
- ⚠️ The compiler no longer supplies its third-party dependencies to add-ins (#321)
- ⚠️ Caching: `[Cache]` and `[InvalidateCache]` are sealed and derive from `MethodLevelAspect`; `CachingServices.DefaultBackend` and `DefaultKeyBuilder` are obsolete (#323)

### New
- `[ImportMember]` on advice parameters (#316)
- `IntroduceFieldAdviceInstance` (#317)
- `ImportDependencyAdviceInstance` and `RunTimeServiceLocator` (#318)
- Caching: `ICachingService` from dependency injection with `[CacheConfiguration( UseDependencyInjection = true )]` (#323)
- `ProjectLocal<T>`, build-time state per project (#315)
- Aspect testing: `.t.cs` snapshots of the woven code with `@CompareTransformedCode` (#326)

### Enhancements
- More advice parameter bindings in `[OnMethodInvokeAsyncAdvice]` (#325)
- `MatchPointcut.IsRequired` reports LA0285 when the advice applies to no method (#328)

### Fixes
- The compiler no longer shares its third-party dependencies with the project being compiled, which fixes PS0264 on version conflicts (#321)

## [2027.0.5-preview] - 2026-10-16

Fifth preview of PostSharp 2027.0, based on 2027.0.4-preview.

### Breaking Changes
- ⚠️ EventSource logging backend: custom activity exits have their own event id, 20 (#304)
- ⚠️ Serilog logging backend: `IndentLevel` is added according to `SerilogSpecialProperties.IndentLevel` (#307)
- ⚠️ Threading: a thread-affine child is checked with the thread-affine compatibility rules (#308)

### New
- The `PostSharp.Testing.AspectTesting` package, an aspect test framework (#272)
- Errors and Warnings reference in the documentation, and corrected texts of 237 messages (#314)

### Fixes
- Aspect framework: aspects on members of referenced assemblies, `InitializeCurrentAspects`, `[CopyCustomAttributes]`, external deserialization constructors, `[MulticastAttributeUsage]` filters, aspect serializers, failing pointcuts, `Message.Write` (#281-#285, #287, #288, #311, #312)
- Compiler and messages: concurrent compilations, message locations, configuration errors, fatal messages, message arguments, AR0106 and AR0110, unreachable report sites (#279, #280, #289-#291, #293, #295, #296)
- Caching: `CachedParameterInfo`, null enumerators, faulted async tasks, profile changes, Redis key prefix, `KeyCompressingThreshold` made obsolete, value adapters (#297-#303)
- Logging: Microsoft backend loggers, `RootSyntheticId`, formatting string exception, `MultiplexerBackend(null)` (#305, #306, #309, #310)
- Threading: THR017 (#292); Model: DOM012 for `Depends.On` in helper methods (#294)
- XAML: XAM051, XAM052, XAM055, XAM077-XAM079, and `Attached<T>.SetValue` (#286, #313)

## [2027.0.4-preview] - 2026-10-14

Fourth preview of PostSharp 2027.0, based on 2027.0.3-preview and PostSharp 2026.0.19, which brings the fixes of PostSharp 2024.0.28.

### New
- ReadyToRun, trimming and NativeAOT (#255): the aspect framework (#89), the pattern libraries (#252), and the `InstantiateTypeAdviceInstance` advice (#254)

### Enhancements
- `PostSharpDependencyRestoreProperties` sets MSBuild properties in the dependency restore projects (#40)
- The licensing, configuration and telemetry services log to the build-time trace (#158)
- Unattended builds are licensed for PostSharp Logging in production mode (#190)
- The `MethodBase` of generic targets is looked up once per generic instance (#242)
- The `FileLock` trace and the `POSTSHARP_FILE_LOCK_*` environment variables take effect (#266)
- Documentation of `MethodPointcut`, `MatchPointcut` and the instance lifecycle advices (#239)

### Fixes
- Runtime async: an exception of `OnYield` or `OnResume` no longer runs the user's `finally` blocks; `PostSharpCompatibilityLevel` 2027.0 routes it through them (#189)
- New diagnostics LA0135, LA0251, LA0252 and LA0254 for advices that were ignored or crashed the build (#244, #258, #259, #249, #263)
- Aspects on C# 14 extension properties are applied (#250)
- Positioned `Require` and `Conflict` messages state the order correctly (#251)
- `[WaiveAspectEffect]` is honored on aspects that use advice methods (#264)
- Advice parameter binding fixes (#256, #257, #260)
- PS0134 on Linux and macOS gives the reason (#265)
- PS0052 on Linux with capital letters in the package version (#268)
- SharpCompress 0.49.0 in the unzipper (#270)
- `VcsCheckEnabled` on Windows no longer waits two minutes and requires a license (#271)
- Attributes on the backing field of an automatic property are reported (#112), and so is an ignored `appliesToMethodOnly` (#267)
- A dependent of an `[AggregateAllChanges]` collection is raised once (#247)
- Closing a logging activity twice is reported as invalid user code (#248)
## [2027.0.3-preview] - 2026-10-08

Third preview of PostSharp 2027.0, based on 2027.0.2-preview.

### Advice Parameter Binding
- The `[Exception]` binding, and `FlowBehavior.ThrowException` in bound exception advices (#164)
- `[ReturnValue]` in method boundary advices (#165)
- The `[Declaration]` binding, which gives the reflection object of the target (#166)
- Advice parameter bindings are checked against a table of the advice kinds that accept them (#163)
- The type arguments of generic advice methods are inferred correctly (#169)
- Breaking: `[CurrentTask]` is `null` until the async method has yielded, and supports `ValueTask` (#238)
- Fixed an internal compiler exception when an advice under `MatchPointcut` binds an argument by name or by type (#243)

### Enhancements
- The MSBuild integration works in the MSBuild server of the .NET 11 SDK (#92)
- `PostSharpValidateLanguageVersion` no longer loads Roslyn into the MSBuild process (#240)

### Breaking Changes
- `TreatWarningsAsErrors` now applies to the warnings of PostSharp and of aspects; set `PostSharpTreatWarningsAsErrors` to `False` for the previous behavior (#201)
- Implementing an `[InternalImplement]` interface of `PostSharp.dll` is now reported as AR0101 (#205)
- Applying an `[Internal]` custom attribute of another assembly is now reported as AR0105 (#206)
- `[Internal]` now reads `[InternalsVisibleTo]` in the right direction (#237)
- `PostSharp.Patterns.Diagnostics.Serilog` now depends on Serilog 4.4.0 (#225)
- The Serilog backend no longer passes string values with quotes (#221)

### Fixes
- Fixed a build failure when the .NET 11 CLI cannot connect to the MSBuild server during the restore of the compiler dependencies (#156)
- `PostSharpExtractTools` no longer extracts the compiler of another package with the same main version (#241)
- Mismatched versions of `PostSharp` and `PostSharp.Redist` are now diagnosed (#193)
- Fixed the public signing flag, the debug directory and the local scopes of the output assembly (#194, #195, #196)
- Fixed missing reference packs and facade assemblies (#197, #198, #199)
- Fixed aspect ordering with a one-sided `Commute`, and PS0114 reporting an aspect as conflicting with itself (#203, #204)
- Fixed threading model defects (#207, #208, #211, #212, #213)
- Fixed `[NotifyPropertyChanged]` defects (#214, #215, #226, #227, #228, #229, #230, #232, #233, #234, #235)
- Fixed `[StrictlyLessThan]` and the other strict inequalities near their bound (#218)
- Fixed the Serilog backend with Serilog 3.0 and later, and type names in custom messages (#220, #222)
- Fixed a process crash when the registry change watcher is disposed during its callback (SharpCrafters.Backstage#23)

### Enhancements of 2024.0.28
These enhancements of 2024.0.28, which is not released yet, are also included.

- PS0265 shows the stack trace of the code that requested an assembly before the project was loaded (#177)
- The `FileLock` trace category records the retries on locked files; the retries themselves are recorded only with a later SharpCrafters.Backstage (#176)
- `POSTSHARP_FILE_LOCK_TIMEOUT` and `POSTSHARP_FILE_LOCK_WARNING` set the retries on locked files; they have no effect until a later SharpCrafters.Backstage (#178)

## [2027.0.2-preview] - 2026-10-03

Second preview of PostSharp 2027.0, based on 2027.0.1-preview.

### New
- Advice parameter binding is now a publicly supported feature, after years of internal use by the Pattern Libraries (#168)
- Added support for runtime-generated async methods (`<Features>runtime-async=on</Features>` on .NET 11) (#187)
- A repository can opt out of telemetry by setting `TelemetryEnabled` to `False` in the `postsharp.config` file at its root (#188, SharpCrafters.Backstage#24)

### Enhancements
- `[SelfPointcut]` and `Master` can be omitted in a declarative aspect when there is only one way to group the advices (#192)

### Fixes
- Fixed an internal compiler exception, or an advice silently disabled, when an advice method has an invalid parameter binding (#161)
- Fixed invalid IL, or an internal compiler exception, when an advice with bound parameters does not match its target (#162)
- The `PostSharp.Tool` package is now published (#171)
- Warnings about the repository configuration are now reported as PS0284 build warnings (#182)
- Fixed usage telemetry slowing down the parallel build of a large solution (SharpCrafters.Backstage#26, metalama/Metalama#2092)
- Fixed a crash when Windows refuses to display a toast notification (SharpCrafters.Backstage#25, metalama/Metalama#2047)

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
