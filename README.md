# dotnet-drmanhatan

`dotnet-drmanhatan` is the .NET member of the DrManhatan observability family.

It expresses the same observability model for C# applications with explicit event construction and protocol timeline tracking.

The package provides a compact runtime for:

- immutable event modeling
- observer-based event delivery
- event enrichment pipelines
- protocol lifecycle telemetry
- session-oriented tracking for stateful channels
- WebSocket-focused convenience helpers in the core runtime

The goal is to make event flow easier to standardize, easier to reason about, and less vulnerable to common implementation mistakes around vendor coupling, protocol lifecycle tracking, retry visibility and message-oriented observability.

## Why Use DrManhatan

`dotnet-drmanhatan` is useful when a system needs observability but should not let transport or telemetry concerns leak into business code.

Typical gains include:

- a single event vocabulary across UI, network and protocol layers
- lower coupling to analytics, logging and monitoring vendors
- easier migration between providers because the domain emits neutral events
- more explicit communication timelines for stateful protocols
- better operational visibility during failures, retries and reconnect flows

## What DrManhatan Does Not Claim

`dotnet-drmanhatan` does not try to replace your network stack, your analytics provider or your monitoring backend.

It does not open WebSocket connections, execute HTTP calls or guarantee that every team will model events with identical naming conventions.

## Repository

- source: [github.com/animalab-netizen/dotnet-drmanhatan](https://github.com/animalab-netizen/dotnet-drmanhatan)

## Coordinates

- package: `dotnet-drmanhatan`
- version: `0.1.1`

Installation:

```bash
dotnet add package dotnet-drmanhatan
```

## Public API

- `Event`
- `IEventObserver`, `DelegateEventObserver`
- `IEventEnricher`, `DelegateEventEnricher`
- `IEventBus`
- `DefaultEventBus`
- `CommonMetadata`
- `CommonMetadataEnricher`
- `HttpError`
- `Protocol`
- `ProtocolEndpoint`
- `ProtocolMessage`
- `ProtocolMessageDirection`
- `ProtocolFailure`
- `ProtocolClose`
- `EventFactory`
- `DrManhatan`
- `ProtocolSessionTracker`
- `WebSocketSessionTracker`

## Basic Example

```csharp
using DotNetDrManhatan;

var bus = new DefaultEventBus();
bus.Subscribe(new DelegateEventObserver(@event =>
{
    Console.WriteLine($"{@event.Name} -> {string.Join(", ", @event.Attributes.Select(item => $"{item.Key}={item.Value}"))}");
}));

var tracker = new DrManhatan(
    bus,
    new EventFactory(
        new CommonMetadata("1.0.0", "dotnet", "prod")
    )
);

tracker.ScreenViewed("Home");
tracker.HttpError("Checkout", new HttpError(500, "server_error"));
```

## Session Example

```csharp
var session = tracker.ProtocolSession(
    Protocol.Mqtt,
    new ProtocolEndpoint("broker"),
    "mqtt-9"
);

session.ConnectionStarted();
session.HeartbeatSent("hb-out-1");
session.ReconnectScheduled(2, 1500, "network_lost");
```

## Validation

```bash
dotnet build
dotnet run --project tests/DotNetDrManhatan.Validation/DotNetDrManhatan.Validation.csproj
```

## Publishing

See [PUBLICATION.md](/Users/caiosanchezchristino/Desktop/drmanhatan-projects/dotnet-drmanhatan/PUBLICATION.md).
