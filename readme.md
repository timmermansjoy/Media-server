# Media server

A media server system for hosting
- Movies
- TV Shows
- Audio books

# Layout

```mermaid
graph TD
    %% Define External Access
    User([End User]) -->|External Access| CF[Cloudflared Tunnel]
    User -->|Internal Access| HP[Homepage]

    %% Routing via Cloudflared
    CF -->|Streams Media| JF[Jellyfin]
    CF -->|Makes Requests| JS[Jellyseerr]
    CF -->|Streams Books| ABS[Audiobookshelf]

    %% Dashboard Connection
    HP -.->|Monitors APIs| All((All Services))

    %% User Requests
    JS -->|Requests TV Shows| S[Sonarr]
    JS -->|Requests Movies| Ra[Radarr]
    JS -->|Checks Existing Library| JF

    %% Quality & Config Management
    Rec[Recyclarr] -.->|Syncs TRaSH Guides| S
    Rec -.->|Syncs TRaSH Guides| Ra

    %% Indexer Management
    P[Prowlarr] -->|Solves Cloudflare Captchas| FS[FlareSolverr]
    P -->|Pushes Indexers| S
    P -->|Pushes Indexers| Ra
    P -->|Pushes Indexers| Re[Readarr]

    %% Downloading
    S -->|Sends .torrent| qB[qBittorrent]
    Ra -->|Sends .torrent| qB
    Re -->|Sends .torrent| qB

    %% Post-Processing
    B[Bazarr] -->|Gets Media Info| S
    B -->|Gets Media Info| Ra

    %% File System / Storage (Shared Volumes)
    subgraph Storage [Shared Docker Volumes]
        Media[("${MEDIA_PATH}")]
    end

    %% Storage Interactions
    qB -->|Downloads to| Media
    S -->|Organizes| Media
    Ra -->|Organizes| Media
    Re -->|Organizes| Media
    B -->|Saves Subtitles to| Media

    %% Media Consumption
    Media -->|Read by| JF
    Media -->|Read by| ABS

    %% Styling
    classDef external fill:#f9f,stroke:#333,stroke-width:2px;
    classDef core fill:#bbf,stroke:#333,stroke-width:2px;
    classDef storage fill:#ff9,stroke:#333,stroke-width:2px;

    class CF,JS,JF,ABS external;
    class S,Ra,Re,P,qB,B core;
    class Media storage;
```


# Hardware encondig

The default docker-compose file is made to work with the rockchip VPU, If you want to run any other devices you can look at [jellyfin's handy manual](https://jellyfin.org/docs/general/administration/hardware-acceleration/)


# Acknowledgements

This project is based of [Morzomb's media server](https://github.com/Morzomb/All-jellyfin-media-server) but more streamlined
