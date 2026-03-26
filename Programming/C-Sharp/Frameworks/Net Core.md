#CSharp #SoftDev 
**NET Core** & its successors (starting from **.NET 5**) are the modern, **cross-platform**, **open-source** evolution of the .[[Net Framework]]. They unify all app types into a single platform.
- init release of **.NET Core**: 2016    
- **.NET 5** (2020): First unified version after .NET Core 3.1    
- Current versions: `.NET 6`, `.NET 7`, `.NET 8` (LTS)    
- Replaces: [[Net Framework]], `Xamarin`, `Mono`

**CoreCLR** - Modern runtime for .NET Core & .NET 5+
- Cross-platform JIT compilation    
- GC, exception handling, threading   
**CoreFX / Base Class Library (BCL)** - Reimplementation of the FCL for cross-platform use
- Lightweight, modular (via NuGet)    
- APIs reside in `System.*` namespaces   
**SDK + CLI Tools** - Includes compilers, project templates, & `dotnet` CLI
- Unified toolchain for all workloads    
- `dotnet build`, `dotnet run`, `dotnet test`, etc.