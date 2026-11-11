# Rede.Compiler

Rede.Compiler compiles analyzed Rede programs into Rede bytecode (`.redebc`). It consumes a high-level, target-independent analyzed program representation whose symbols, types, and operations have already been resolved by a language frontend.

## Role in the toolchain

- **Rede.Language** tokenizes, parses, and semantically analyzes Rede source. It produces an analyzed program representation and uses package declarations to understand exported names, types, signatures, and semantics without knowing package implementations.
- A **Semantic Development Environment**, such as Reder, orchestrates the toolchain and passes a suitable analyzed program representation directly to Rede.Compiler. Its own semantic model and derived views are separate from that representation.
- **Rede.Compiler** validates the analyzed input, uses internal representations as needed, and generates and serializes bytecode. Optional plugins handle package-specific compilation behavior; ordinary package calls can be represented as external references.
- A **target backend or runtime** consumes bytecode and resolves or implements package references. Different targets can support the same bytecode if they implement its semantics and required package capabilities. Target-specific compilation, linking, and hosting are separate from the compiler's bytecode generation.

Other frontends or tools can produce compiler input independently, provided they satisfy the analyzed representation's contract. The components can evolve independently, subject to the compatibility rules of their shared contracts.

## Compiler pipeline

```text
Analyzed program → input validation → lowering → bytecode generation
				 → bytecode validation → .redebc serialization
```

The analyzed representation carries resolved symbol identities, types, operations and operands, control-flow constructs and their language meaning, package references, and source locations where available. The compiler validates its input, reports malformed or unsupported programs through structured diagnostics, and distinguishes suitable analysis results from partial results containing errors. Source analysis and environment orchestration remain outside the compiler core.

The analyzed representation, plugin API, bytecode format and semantics, and package-reference rules have compatibility and versioning contracts. Package declarations describe what source programs can use; package implementations and target-specific bindings are separate. Plugin code runs with the host's privileges and is treated as trusted executable code.

## Responsibilities

```text
Compilation/   Public compiler entry point, options, and results
Input/         Analyzed representation consumption and input validation
Lowering/      Translation to a bytecode-oriented form
Bytecode/      Bytecode model, generation, validation, and serialization
Plugins/       Optional package-specific compilation contracts
Diagnostics/   Structured diagnostics and source locations
```

These are areas of responsibility rather than mandatory namespaces or projects.

## Requirements

- .NET 10 SDK

## Build

```sh
dotnet build Rede.Compiler.slnx
```
