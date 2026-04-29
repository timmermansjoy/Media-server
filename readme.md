# Media server

A media server system for hosting
- Movies
- TV Shows
- Audio books

# Layout

```mermaid
graph TD
    %% ==========================================
    %% 1. Node Definitions (The "Things")
    %% ==========================================
    User([End User])

    %% -- Ingress/Access Layer --
    HP(Homepage<br/>Dashboard)
    CF{Cloudflared<br/>Tunnel}

    %% -- Request/Discovery Layer --
    JS(Jellyseerr<br/>Requests)

    %% -- Management Layer ("The Arrs") --
    Arrs(Media Managers<br/>Sonarr | Radarr | Readarr)
    B(Bazarr<br/>Subtitles)
    Rec((Recyclarr<br/>Configs))

    %% -- Download Layer --
    qB[qBittorrent<br/>Downloader]
    P(Prowlarr +<br/>FlareSolverr)

    %% -- Storage Layer --
    Media[(Shared Volume<br/>${MEDIA_PATH})]

    %% -- Consumption Layer --
    Players(Media Players<br/>Jellyfin | Audiobookshelf)


    %% ==========================================
    %% 2. Connections & Arrows (The "Flow")
    %% ==========================================

    %% User Access
    User ==>|Internal| HP
    User ==>|External| CF

    %% Access -> Frontend
    CF --> JS
    CF --> Players

    %% Discovery -> Logic
    JS -->|Sends Request| Arrs

    %% Logic -> Configuration
    Rec -.->|Syncs Guides| Arrs

    %% Logic -> Indexing/Downloading
    Arrs -->|Searches| P
    Arrs -->|Pushes Torrent| qB

    %% Post-Processing
    B -->|Scans Media| Arrs

    %% Data Flow to/from Storage
    qB ==>|Writes Data| Media
    Arrs ==>|Renames/Moves| Media
    B -->|Saves Subtitles| Media

    %% Storage to Players
    Media ==>|Streams Data| Players


    %% ==========================================
    %% 3. Professional, Readable Styling
    %% ==========================================
    %% (This section ensures visibility in both Light & Dark modes)

    %% Define Class Styles (Colors and Borders)
    classDef default fill:#f9f9f9,stroke:#333,stroke-width:2px,color:black,rx:5,ry:5;
    classDef user fill:#e1f5fe,stroke:#0277bd,stroke-width:2px,color:#01579b,font-weight:bold;
    classDef access fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#4a148c;
    classDef manager fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#1b5e20;
    classDef download fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,color:#e65100;
    classDef storage fill:#fffde7,stroke:#fbc02d,stroke-width:2px,color:#f57f17;
    classDef consumer fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#b71c1c;

    %% Apply Classes to Nodes
    class User user;
    class HP,CF access;
    class JS,Arrs,B,Rec manager;
    class qB,P download;
    class Media storage;
    class Players consumer;

    %% Thicken all lines for readability
    linkStyle default stroke:#555,stroke-width:1.5px;
    %% Bold key data paths
    linkStyle 1,2,9,10,12 stroke:#333,stroke-width:3px;
```


# Hardware encondig

The default docker-compose file is made to work with the rockchip VPU, If you want to run any other devices you can look at [jellyfin's handy manual](https://jellyfin.org/docs/general/administration/hardware-acceleration/)


# Acknowledgements

This project is based of [Morzomb's media server](https://github.com/Morzomb/All-jellyfin-media-server) but more streamlined
