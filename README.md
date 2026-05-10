# mTorrentAtlas

A mobile-oriented distributed file sharing system with server-coordinated chunk streaming.

Files are split into fixed-size chunks on upload, distributed across storage nodes with replication, and reassembled on the client after download. The coordinator server acts as the control plane — it holds metadata and orchestrates transfers but stores no file data itself.

## System Overview

```
Flutter Client  ──TCP──►  Coordinator Server  ──TCP──►  Storage Node 1
                                                    └──►  Storage Node 2
                                                    └──►  Storage Node N
```

- **Coordinator** manages file metadata, chunk placement, and replication
- **Storage Nodes** store chunk files and serve them to the coordinator on demand
- **Client** uploads/downloads files and reconstructs them locally

## Architecture

```text
┌─────────────────────┐        TCP Socket        ┌──────────────────────┐
│   Flutter Client    │ ◄──────────────────────► │  Coordinator Server  │
│                     │   Custom Text Protocol   │        (C++)         │
└─────────────────────┘                          └──────────┬───────────┘
                                                            │
                               TCP Socket Connections       │
                                                            ▼
                         ┌───────────────────────────────────────┐
                         │            Storage Nodes              │
                         ├───────────────────────────────────────┤
                         │  Storage Node 1                       │
                         │  Storage Node 2                       │
                         │  Storage Node N                       │
                         └───────────────────────────────────────┘
```

## Repositories

| Repo | Description |
|---|---|
| [`mTorrentNexus`](https://github.com/M-AffanK/mTorrentNexus.git) | C++ coordinator server and storage node |
| [`mTorrent`](https://github.com/M-AffanK/mTorrent.git) | Flutter mobile client |

## Quick Start

- **Coordinator Server** → `backend/`
- **Storage Nodes** → `backend/`
- **Mobile Client** → `app/`

## Tech Stack

- **Server / Storage Node** — C++17, POSIX sockets, pthreads
- **Mobile Client** — Flutter / Dart
