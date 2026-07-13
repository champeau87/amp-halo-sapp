# AMP Halo Custom Edition + SAPP — Safe Templates v9

v9 fixes a destructive configuration bug in v8.

## What went wrong in v8

`SAPP_CE/cg/init.txt` was declared multiple times in the metaconfig. AMP treated
each declaration as a writer for the whole target file. The final declaration
contained only:

    sv_maxplayers
    sv_public
    sv_mapcycle_timeout

so the other lines were removed.

## Immediate recovery

Before restarting, restore `SAPP_CE/cg/init.txt` to at least:

    sv_name "Your Server Name"
    sv_maxplayers 8
    sv_public 0
    sv_mapcycle_timeout 3
    sv_rcon_password ChangeMe

    load
    mapcycle_begin

Replace the name and password with your real values.

## v9 design

There is now exactly one AMP template owner for each generated file:

    SAPP_CE/cg/init.txt
    SAPP_CE/cg/sapp/init.txt

The source templates are:

    AMP_init.txt
    AMP_sapp_init.txt

AMP downloads those source templates from this GitHub repository during Update,
then expands `{{SettingName}}` placeholders into the two target files.

The repository therefore must contain:

    halo-ce-AMP-init.txt
    halo-ce-AMP-sapp-init.txt

in addition to the normal module files.

## Required fixed Halo commands

The generated Halo init always retains:

    load
    mapcycle_begin

`load` activates SAPP. `mapcycle_begin` starts the packaged SAPP map cycle.

## Custom commands

The AMP GUI now includes:

- Additional Halo Startup Commands
- Additional SAPP Commands

Use those fields for commands not yet represented by dedicated AMP controls.
Do not hand-edit the generated target files because AMP will regenerate them.

## Apply v9

1. While the server is stopped, manually repair `SAPP_CE/cg/init.txt` using the
   recovery content above.
2. Replace the GitHub repository files with all files from this package.
3. Push to `main`.
4. Fetch `champeau87/amp-halo-sapp:main` in ADS.
5. Apply the updated template to the Halo instance.
6. Run the instance's **Update** action so AMP downloads:
   - `AMP_init.txt`
   - `AMP_sapp_init.txt`
7. Open the settings page and save once.
8. Inspect:
   - `SAPP_CE/cg/init.txt`
   - `SAPP_CE/cg/sapp/init.txt`
9. Confirm `load` and `mapcycle_begin` remain before starting.

## Expected Halo init

After saving, it should resemble:

    sv_name "AMP Halo CE Server"
    sv_maxplayers 8
    sv_public 0
    sv_mapcycle_timeout 3
    sv_rcon_password ChangeMe

    load
    mapcycle_begin
