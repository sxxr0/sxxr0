# Hi, I'm Michal

**C# developer** drawn to the low-level side of .NET: runtime hosting, IL, and mod loaders for Unity games.  
Writing C# for 6+ years. Software Development student (MBO), originally from Poland, based in the Netherlands, and currently open to internship opportunities.

---

## What I'm working on

### [Lustral](https://github.com/LustralTeam/Lustral)
**A next-generation mod loader for Unity games, built from scratch.** After working on MelonLoader's internals, I started Lustral to solve the problems that the established loaders, MelonLoader and BepInEx, leave to modders and players: library conflicts between mods, broken patches that only show up at launch, manual project setup, and mod updates left to external tools. It supports both Mono and IL2CPP games, and the first release (v0.1.0) is out in early testing. [Documentation](https://lustralteam.github.io/Lustral.Docs/)

- **Mods don't fight over libraries.** On IL2CPP games each mod runs in its own `AssemblyLoadContext`, so two mods can ship different versions of the same library. When a mod fails to load, its Harmony patches are rolled back and the mods that depend on it are skipped, and crash reports name the mods involved.
- **Broken patches are caught at build time, not at launch.** Roslyn analyzers check every Harmony patch against the game's real assemblies during the build, flagging missing target methods, ambiguous overloads and mismatched parameters.
- **Zero-setup mod projects.** An MSBuild SDK and `dotnet new` template find the game, reference its assemblies, generate the manifest and deploy the mod on every build. The same project can also build for both Mono and IL2CPP without any manual multi-targeting.
- **Updates built into the loader.** Mods update themselves from GitHub releases or JSON feeds on the next launch, with automatic rollback if an update fails. No separate mod manager needed.
- **A modern IL2CPP stack.** IL2CPP mods run on .NET 10 against interop assemblies generated on first launch with Il2CppInterop V2, the rewrite I contribute to. Custom Unity components are declared with an attribute and registered automatically, instead of through manual `ClassInjector` calls.

**Under the hood:** a native C++ `winhttp.dll` proxy intercepts Unity's runtime startup (`il2cpp_init` / `mono_jit_init_version`), then either boots CoreCLR directly through `coreclr_initialize`, skipping hostfxr for full control over the runtime, or loads into the game's own Mono domain. Covered by unit and end-to-end tests, with CI and releases on GitHub Actions.

### [Il2CppInterop V2](https://github.com/ds5678/Il2CppInterop/tree/v2-rewrite)
Il2CppInterop bridges Unity's IL2CPP runtime and .NET's CoreCLR, and is the interop layer behind IL2CPP modding in BepInEx and MelonLoader. I contribute to its V2 rewrite, with 24 merged pull requests so far; Lustral's IL2CPP backend is built on it.

- **Class injection:** made managed types injected into IL2CPP behave like native ones, including their type hierarchy ([#25](https://github.com/ds5678/Il2CppInterop/pull/25)), vtable and interface layout ([#36](https://github.com/ds5678/Il2CppInterop/pull/36)), structs ([#26](https://github.com/ds5678/Il2CppInterop/pull/26)), enums ([#32](https://github.com/ds5678/Il2CppInterop/pull/32), [#33](https://github.com/ds5678/Il2CppInterop/pull/33)) and static fields ([#41](https://github.com/ds5678/Il2CppInterop/pull/41))
- **Garbage collection:** injected objects now report their references to IL2CPP's garbage collector ([#23](https://github.com/ds5678/Il2CppInterop/pull/23)), and references stored in the IL2CPP heap go through its write barrier ([#24](https://github.com/ds5678/Il2CppInterop/pull/24))
- **Generator and runtime fixes:** IL translation of unstripped method bodies ([#19](https://github.com/ds5678/Il2CppInterop/pull/19)), primitive types in native calls ([#27](https://github.com/ds5678/Il2CppInterop/pull/27)), loading interop assemblies on demand ([#31](https://github.com/ds5678/Il2CppInterop/pull/31))
- **Documentation** for assembly generation, runtime startup and Harmony support in V2 ([#21](https://github.com/ds5678/Il2CppInterop/pull/21))

All my pull requests: [Il2CppInterop V2](https://github.com/ds5678/Il2CppInterop/pulls?q=is%3Apr+author%3Asxxr0)

---

## Before this account: MelonLoader

My earlier work lives on [@slxdy](https://github.com/slxdy), where I contributed to [MelonLoader](https://github.com/LavaGang/MelonLoader), a mod loader for Unity games (Mono and IL2CPP) with 4k+ ★.

- **Bootstrap rewrite:** rewrote the native bootstrap from Rust to C# compiled with NativeAOT, so the whole loader, down to its native entry point, is written in C#
- **Simpler configuration:** moved loader settings from command-line arguments into a plain config file
- **[MelonLoader.Installer](https://github.com/LavaGang/MelonLoader.Installer):** rewrote the official installer from scratch, moving it from WinForms to Avalonia and adding automatic game detection
- **[MelonLoader.UnityDependencies](https://github.com/LavaGang/MelonLoader.UnityDependencies):** built a GitHub Actions pipeline that collects Unity runtime libraries, which MelonLoader uses for assembly generation

My pull requests: [MelonLoader](https://github.com/LavaGang/MelonLoader/pulls?q=is%3Apr+author%3Aslxdy) · [MelonLoader.Installer](https://github.com/LavaGang/MelonLoader.Installer/pulls?q=is%3Apr+author%3Aslxdy)

---

## Toolbox

**Languages:** C# · C++ · HTML/CSS/JS · PHP  
**.NET internals:** CoreCLR & Mono hosting · NativeAOT · AssemblyLoadContext · IL (Mono.Cecil, AsmResolver)  
**Unity modding:** IL2CPP & Mono internals · Il2CppInterop · Cpp2IL · HarmonyX  
**.NET tooling:** MSBuild SDKs · Roslyn analyzers · xUnit  
**Desktop:** Avalonia · WPF  
**Reverse engineering:** IL, x86-64 assembly  
**Build & CI:** CMake · GitHub Actions

---

**Contact:** [sxxr0dev@gmail.com](mailto:sxxr0dev@gmail.com)
