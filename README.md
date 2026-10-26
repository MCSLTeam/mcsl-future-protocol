# MCSL Future V2 protocol documentation

This Writerside site explains the current .NET daemon V2 protocol. It is not an
independent protocol definition or a compatibility specification for the experimental
Rust daemon. The removed daemon V1 endpoint and action/retcode envelopes are not
supported.

## Source of truth

This alignment was reviewed against public .NET commit
[`8490ffd9e5c543c2ab8e71fd6b9acd6d35729233`](https://github.com/MCSLTeam/MCServerLauncher-Future/tree/8490ffd9e5c543c2ab8e71fd6b9acd6d35729233):

- [Built-in RPC/event definitions](https://github.com/MCSLTeam/MCServerLauncher-Future/blob/8490ffd9e5c543c2ab8e71fd6b9acd6d35729233/src/MCServerLauncher.Daemon.API/Protocol/BuiltInProtocolDefinitions.cs)
- [Shared wire contracts](https://github.com/MCSLTeam/MCServerLauncher-Future/tree/8490ffd9e5c543c2ab8e71fd6b9acd6d35729233/src/MCServerLauncher.Common/Contracts/Protocol)
- [Daemon protocol topics](https://github.com/MCSLTeam/MCServerLauncher-Future/tree/8490ffd9e5c543c2ab8e71fd6b9acd6d35729233/src/MCServerLauncher.Daemon/.Resources/Docs/protocol/topics)

The daemon's frozen typed catalog is authoritative. On an authenticated `/api/v2`
WebSocket connection, `rpc.discover` returns runtime OpenRPC including admitted
plugins. HTTP `GET /apifox.json` exports the same catalog as an Apifox project.
The checked-in Apifox baseline in the .NET repository contains built-ins only.

This site intentionally links consumers to catalog-derived schemas instead of
maintaining another complete hand-written method, permission, or DTO registry.
The existing `action.md`, `actions.md`, and `action-errcode.md` filenames are retained
for existing page URLs; their titles and contents now describe V2 JSON-RPC, not V1.

## Maintaining the site

1. Review changes against the .NET catalog, wire codec, and embedded protocol topics.
   Record a new reviewed source commit here when realigning the site.
2. Update explanations and examples together. Do not restore V1 fallbacks or replace
   every `v1` string: binary header version `1`, extension subprotocol versions, and
   the separate plugin Registry API are independent version axes.
3. In the .NET repository, verify the generated baseline without rewriting it:

   ```bash
   dotnet run --project tools/MCServerLauncher.ProtocolDocs/MCServerLauncher.ProtocolDocs.csproj -- --check
   ```

4. Check topic links, JSON examples, and the Writerside table of contents. The
   `.github/workflows/build.yml` workflow builds and checks Writerside output before
   Pages deployment; do not trigger publishing just to validate a local edit.

The Writerside instance's product version is not a daemon wire or SDK package version.
