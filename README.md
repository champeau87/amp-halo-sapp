# AMP Halo Custom Edition + SAPP — Managed Settings v6

This release keeps the working Wine/SAPP launch configuration from v5 and adds
AMP settings panels for the most useful Halo and SAPP options.

## Managed in AMP

### Halo Server

- Server name
- Maximum players
- Public/private server listing
- Map-cycle timeout
- RCON password

### SAPP Settings

- No-lead
- SAPP console mode
- Chat output in AMP console
- Chat anti-spam
- AFK kicking
- High-ping kicking
- SAPP logging

### Map Cycle

- SAPP map-cycle enablement
- Map voting enablement

The actual map lists remain in:

    SAPP_CE/cg/sapp/mapcycle.txt
    SAPP_CE/cg/sapp/mapvotes.txt

They should still be edited through AMP File Manager for now.

## Safety design

AMP edits the existing files rather than replacing them:

    SAPP_CE/cg/init.txt
    SAPP_CE/cg/sapp/init.txt

The parsers are restricted to specific command names. Unmanaged lines such as:

    load
    mapcycle_begin

should remain intact.

## Applying v6 to the running instance

1. Stop the Halo instance.
2. Back up:
   - `SAPP_CE/cg/init.txt`
   - `SAPP_CE/cg/sapp/init.txt`
3. Replace the files in the GitHub template repository and push to `main`.
4. In ADS, fetch `champeau87/amp-halo-sapp:main`.
5. On the main ADS instance list, use the instance's **Update** action to refresh
   its Generic Module template.
6. Open the Halo instance and review the new settings categories before starting.
7. Change the default RCON password immediately.
8. Start the instance.
9. Confirm that `sv_status` works and inspect both init files to make sure the
   settings were written once without duplicated commands.

Do not use the Support and Updates page for the template refresh. Generic Module
template configurations are refreshed from the instance action on the ADS list.

## Restart behavior

These settings are file-backed and are not marked for live updates. Restart the
server after changing them.

## Next management layer

After this version is validated, the next sensible additions are:

- AMP player join/leave/chat recognition from real console output
- application-ready detection instead of immediate readiness
- custom AMP actions for status, next map, reload SAPP, and server messages
- a safer map-cycle editor
- admins and ban-management workflows
