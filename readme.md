# Media server

A media server system for hosting
- Movies
- TV Shows
- Audio books

# Layout

```mermaid
graph TD
    %% Access Layer
    User([End User]) -->|External| CF[Cloudflared Tunnel]
    User -->|Internal| HP[Homepage Dashboard]

    %% Front-End Services
    CF -->|Requests Media| JS[Jellyseerr]
    CF -->|Streams Media| Players[Players: Jellyfin & Audiobookshelf]

    %% Management Layer
    JS -->|Sends Approvals| Arrs[Media Managers: Sonarr, Radarr, Readarr]
    Recyclarr -.->|Syncs TRaSH Profiles| Arrs

    %% Search & Download Layer
    Arrs -->|Searches Indexers| Prowlarr[Prowlarr + FlareSolverr]
    Arrs -->|Sends Torrents| qB[qBittorrent]

    %% Storage Layer
    subgraph Storage [Shared Docker Volumes]
        Media[("${MEDIA_PATH}")]
    end

    %% Data Flow
    qB -->|Downloads| Media
    Arrs -->|Renames & Moves| Media
    Bazarr -->|Downloads Subtitles| Media

    %% Consumption
    Media -->|Read by| Players

    %% Styling
    classDef external fill:#f9f,stroke:#333,stroke-width:2px;
    classDef core fill:#bbf,stroke:#333,stroke-width:2px;
    classDef storage fill:#ff9,stroke:#333,stroke-width:2px;

    class CF,JS,Players external;
    class Arrs,Prowlarr,qB,Bazarr,Recyclarr core;
    class Media storage;
```


# Hardware encondig

The default docker-compose file is made to work with the rockchip VPU, If you want to run any other devices you can look at [jellyfin's handy manual](https://jellyfin.org/docs/general/administration/hardware-acceleration/)


# Acknowledgements

This project is based of [Morzomb's media server](https://github.com/Morzomb/All-jellyfin-media-server) but more streamlined
