# IMPORTANT: upload these files directly

Use GitHub **Add file > Upload files** and upload the extracted files. Do not copy/paste the KVP contents into GitHub's editor. The included `.gitattributes` keeps the required LF line endings.

# AMP Halo Custom Edition + SAPP — Package-Aware Settings v10

v10 retains the working pseudo-terminal console from v7 and fixes the settings
mapping against Chalwk's actual `SAPP_CE.zip` configuration files.

## What the pasted files revealed

The package uses:

    sv_public true
    mapvote false
    no_lead true

not numeric `1` and `0` values.

It also does not include `sv_maxplayers` in `cg/init.txt`, and several optional
SAPP settings are absent from `cg/sapp/init.txt`. AMP AutoMap updates existing
keys; it does not reliably invent missing lines.

## What Update now does

The **Prepare SAPP Configuration for AMP** stage safely adds these only when
they are missing:

In `SAPP_CE/cg/init.txt`:

    sv_maxplayers 16

In `SAPP_CE/cg/sapp/init.txt`:

    console_input true
    chat_console_echo true
    antispam 0
    afk_kick 0
    ping_kick 0
    log false

The existing vendor comments and unmanaged settings are preserved.

## Correct setting locations

`SAPP_CE/cg/init.txt`:

    sv_name
    sv_public
    sv_rcon_password
    sv_maxplayers

`SAPP_CE/cg/sapp/init.txt`:

    sv_mapcycle_timeout
    no_lead
    console_input
    sapp_console
    chat_console_echo
    sapp_mapcycle
    mapvote
    antispam
    afk_kick
    ping_kick
    log

## Map cycle warning

SAPP Map Cycle and Map Voting are mutually exclusive. For voting:

    SAPP Map Cycle: Off
    Map Voting: On

For automatic rotation:

    SAPP Map Cycle: On
    Map Voting: Off

## Apply v10

1. Stop the Halo instance.
2. Back up:
   - `SAPP_CE/cg/init.txt`
   - `SAPP_CE/cg/sapp/init.txt`
3. Replace all repository module files with v10 and push to `main`.
4. Fetch `champeau87/amp-halo-sapp:main` in ADS.
5. Apply the updated template to the existing instance.
6. Run the instance **Update** action once.
7. Reopen the Halo Server settings, set the desired values, and save.
8. Inspect both init files before starting.

## Expected result for the current test

With:

    Server Name: terst
    Maximum Players: 8
    Public Server: Off
    RCON Password: party
    SAPP Map Cycle: Off
    Map Voting: On

`SAPP_CE/cg/init.txt` should contain:

    sv_name "terst"
    sv_public false
    sv_rcon_password "party"
    sv_maxplayers 8

`SAPP_CE/cg/sapp/init.txt` should contain:

    sapp_mapcycle false
    mapvote true

The rest of the vendor configuration should remain in place.
