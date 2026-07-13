# AMP Halo Custom Edition + SAPP — Explicit Config Mapping v8

v8 keeps the working PTY console from v7 and fixes configuration-file mapping.

## Why this release exists

The server-name setting was being written successfully, but the grouped numeric
settings such as `sv_maxplayers` and `sv_public` were not reliably updating the
file.

v8 replaces the automatic mapping with explicit mappings:

    sv_name             -> ServerName
    sv_maxplayers       -> $MaxUsers
    sv_public           -> PublicServer
    sv_mapcycle_timeout -> MapcycleTimeout
    sv_rcon_password    -> $RemoteAdminPassword

The parser also accepts trailing `; comments` and extra whitespace.

## Correct files

Halo startup settings are written to:

    SAPP_CE/cg/init.txt

SAPP-specific settings are written to:

    SAPP_CE/cg/sapp/init.txt

`sv_maxplayers` and `sv_public` do not belong in the SAPP init file.

## Test procedure

1. Stop the server.
2. Back up:
   - `SAPP_CE/cg/init.txt`
   - `SAPP_CE/cg/sapp/init.txt`
3. Install/fetch the v8 template.
4. Apply the updated template to the existing instance.
5. Set:
   - Maximum Players: `8`
   - Public Server: `Off`
6. Save the settings.
7. Before starting the server, open:

       SAPP_CE/cg/init.txt

8. Confirm that it contains:

       sv_maxplayers 8
       sv_public 0

9. Start the server.
10. In AMP Console, query the live values:

       sv_maxplayers
       sv_public

11. Restore the desired production values after the test.

## Expected restart behavior

These are startup-file settings. AMP writes the file when settings are saved,
but the running Halo process will not use the new values until the server is
restarted, unless the commands are also entered manually in the live console.
