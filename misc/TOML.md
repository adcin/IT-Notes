# TOML - Tom's Obvious, Minimal Language

> [!quote] Quote from [toml.io](https://toml.io/en/)
> 
> **A config file format for humans.**
> 
> TOML aims to be a minimal configuration file format that's easy to read due to obvious semantics. TOML is designed to map unambiguously to a hash table. TOML should be easy to parse into data structures in a wide variety of languages.

- [toml.io](https://toml.io)
- [toml.io - specs v1.0.0](https://toml.io/en/v1.0.0), [Text Version (MarkDown)](https://raw.githubusercontent.com/toml-lang/toml.io/main/specs/en/v1.0.0.md)
- [wikipedia.org - TOML](https://wikipedia.org/wiki/TOML)
- [GitHub.com - toml-lang/toml](https://github.com/toml-lang/toml)

## TOML syntax

> [!quote] Example from [toml.io]
> 
> ```TOML
> # This is a TOML document
> 
> title = "TOML Example"
> 
> [owner]
> name = "Tom Preston-Werner"
> dob = 1979-05-27T07:32:00-08:00
> 
> [database]
> enabled = true
> ports = [ 8000, 8001, 8002 ]
> data = [ ["delta", "phi"], [3.14] ]
> temp_targets = { cpu = 79.5, case = 72.0 }
> 
> [servers]
> 
> [servers.alpha]
> ip = "10.0.0.1"
> role = "frontend"
> 
> [servers.beta]
> ip = "10.0.0.2"
> role = "backend"
> ```

