# Readme
This is intended as a starting point for setting up a media tracking stack.  It contains:

- [Sonarr](https://sonarr.tv/)  - TV show tracker
- [Radarr](https://radarr.video/) - Movie tracker
- [Bazarr](https://www.bazarr.media/) - Subtitle Tracker
- [NZBGet](https://nzbget.net/) - Usenet downloader
- [Syncthing](https://syncthing.net/) - File Synchronisation - local to remote and vice versa
- [Jackett](https://github.com/Jackett/Jackett) - Torrent Tracker interface for Sonarr / Radarr
- [Jellyfin](https://jellyfin.org/) - Browser-based remote media player

##  Prerequisites

 - Docker and Docker-Compose
 - Local storage space for your desired media
 - Membership to a supported NZB indexer in order to obtain NZBs
 - Membership to a supported usenet provider in order to use NZBGet
 - Membership to a supported tracker in order to use Jackett
 - A remote Syncthing instance to utilise remote file syncing
## Basic Configuration
1. Create a directory on your device where you intend to store your tracked media
2. `cp .env.example .env`
3. Edit `.env` and point `CONTAINER_DATA` to your created directory
4. Set the `TIMEZONE` to your desired timezone
5. Save the `.env` file
## How to Run
From the repository root, `docker-compose up` to bring the stack up.  Note that this will run in the foreground.  If you kill the process then the stack will be taken down.  To run the stack in the background use `docker-compose up -d`.  You can monitor the stack by using `docker-compose logs`.
## Configuring Each Service
### NZBGet
- Visit `http//localhost:6789` and complete the initial configuration
- Set your downloads directory to `/downloads`
- Add your preferred Usenet providers
### Jackett (Optional)
- Visit http://localhost:9117 and complete the initial configuration
- Set the blackhole directory to `/downloads`
- Configure each tracker you wish to connect to individually.  Consult Jackett's documentation for more detailed information as to how to do this.
### Syncthing (Optional)
This service is only useful if you have a remote syncthing instance that you are using to sync to local.  The remote syncthing can utilise a synced local `watch` directory, and the local syncthing can utilise a synced remote `downloads` directory.  The remote Syncthing should already have a "downloads" share ready to share.

- Visit https://localhost:8384 (ignore https warnings) to configure.
- Name your local syncthing instance.
- Add a share for your `/watch` directory.  Name it however you wish, set it to `Send Only`, and allow it to be advertised to peers.
- Copy the local Syncthing server's UUID.
- On the remote Syncthing server, add a peer.  Use the UUID of  your local Syncthing.  Be sure to share the "downlads" share with the peer.
- Once the peer has been found, the remote Syncthing server should show a notification asking to add the `watch` share.  Add this share in the desired directory on the remote Syncthing server.
- The local Syncthing server should show a notification asking to add the "downloads" share.  Add this share as a `Receive Only` share, and use the `/downloads` directory.
### Bazarr
- Visit http://localhost:6767 to initialise configuration.
- Add the desired subtitle providers
- Note Bazarr's API key
### Sonarr
- Visit https://localhost:8989 to initialise configuration
- 

