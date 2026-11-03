Obsidian LiveSync — Self-Hosted Sync Deployment (2026-11-03)

Scope

Built a self-hosted Obsidian synchronization backend for a cross-device Markdown knowledge base. Backend verified; device-to-device LiveSync replication still pending. This is an operational record, not a claim of completed client synchronization.

Architecture

Obsidian (Linux / iOS / iPadOS; test vaults pending)
       | HTTPS (VPN for remote access)
Private DNS record -> Caddy reverse proxy / internal TLS
       | HTTP on restricted internal network
Dedicated unprivileged Debian 12 LXC (Proxmox VE)
       | Docker Compose
Apache CouchDB 3.5.3 + persistent bind-mounted storage

• Dedicated, unprivileged Proxmox LXC with automatic startup, 1 vCPU, 1 GiB RAM, 512 MiB swap and a 16 GiB local-lvm volume.
• Isolated service on Servers VLAN, avoiding the production DNS VM and admin workstation.
• Docker Engine installed from Docker’s Debian repository; Compose and hello-world verified.
• CouchDB image pinned to a known version, with persistent data/config mounts, restart policy, and rotating container logs.
• CouchDB credentials generated locally, held in a root-readable-only .env, and excluded from Git. An inadvertently disclosed credential was rotated; authenticated access subsequently passed.
• LiveSync CouchDB initialization utility run with Deno, configuring application CORS, authentication and the disposable test database.
• Private DNS points the application’s hostname at Caddy (not directly at CouchDB). Caddy proxies HTTPS requests to the CouchDB upstream through an explicitly authorized internal connection.
• Initial Caddy 502 identified as backend reachability/firewall policy; after correcting the allowlist, HTTPS returned expected HTTP 401 for an unauthenticated request.
• iPhone Safari independently confirmed HTTPS connectivity to CouchDB’s welcome endpoint. Browser connectivity does not demonstrate Obsidian replication.

Verification evidence

|Check                                           |Observed result                         |
|------------------------------------------------|----------------------------------------|
|LXC boot, VLAN gateway, DNS, external network   |Passed                                  |
|Docker daemon and Compose                       |Active; hello-world passed              |
|CouchDB endpoint                                |HTTP 200 with CouchDB 3.5.3 welcome JSON|
|CouchDB test database                           |Present                                 |
|Unauthenticated `/_all_dbs`                     |HTTP 401                                |
|Password-manager value vs protected `.env`      |Matched                                 |
|Password in `.env` authenticated against CouchDB|HTTP 200                                |
|HTTPS through Caddy                             |Expected HTTP 401 for protected endpoint|
|iPhone browser test                             |CouchDB welcome JSON visible            |
|Client LiveSync replication                     |**Not yet tested**                      |

Remaining actions

1. Verify that both existing mobile vaults are backed up. Keep them separate; do not overwrite or merge.
2. On an available admin workstation, create a new local LiveSync-Test Obsidian vault, not in another sync-managed folder.
3. Install Self-hosted LiveSync, configure the existing disposable CouchDB database, and save a separate encryption passphrase in the password manager.
4. Create and synchronize a test note, then use the plugin’s setup URI to connect a new empty test vault on iPhone, then iPad.
5. Confirm two-way sync, offline edits, conflict behavior, deletion/rename behavior, and VPN access from outside the LAN.
6. Design and test independent versioned backups and restoration. The original backup target had insufficient free capacity and must be remediated first.
7. Only after successful tests, connect a verified backup/copy of the real Markdown vault to a separate production database.

Security and operational notes

• Never commit .env, database credentials, encryption passphrases, generated setup URIs, private CA keys or production databases.
• No public exposure of CouchDB port 5984. Limit upstream access to the reverse proxy and approved administration sources; use VPN for remote devices.
• An unprivileged LXC containing Docker still requires host-level trust and careful updates.
• Internal TLS must be trusted by every mobile device; do not disable certificate verification.
• Git documents intent and configuration history; CouchDB replication is not a backup.
• Keep operational IP addresses, firewall object IDs and internal host mapping details in a protected private source of truth, rather than this public portfolio.