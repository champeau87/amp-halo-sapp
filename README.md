# AMP Halo Custom Edition + SAPP

This is an AMP Generic Module for the ready-to-run `SAPP_CE.zip` package from
Chalwk/SPCLib.

## Files that belong in the repository

- `halo-ce.kvp`
- `halo-ceconfig.json`
- `halo-cemetaconfig.json`
- `halo-ceports.json`
- `halo-ceupdates.json`
- `manifest.json`

`README.md` is documentation and is safe to keep in the repository.

## What AMP installs

The first AMP Update downloads and extracts:

https://github.com/Chalwk/SPCLib/releases/download/sapp-server-templates/SAPP_CE.zip

The bootstrap download and Wine-prefix initialization are marked `OneShot`.
They are intended to run once for each new AMP instance.

The archive contains a top-level `SAPP_CE` directory, so AMP extracts it as:

    halo-ce/serverfiles/SAPP_CE/

The expected files include:

    SAPP_CE/haloceded.exe
    SAPP_CE/sapp.dll
    SAPP_CE/Strings.dll
    SAPP_CE/run.bat
    SAPP_CE/run.sh
    SAPP_CE/cg/init.txt
    SAPP_CE/cg/sapp/init.txt
    SAPP_CE/maps/
    SAPP_CE/sapp/

AMP deliberately keeps that vendor directory intact. The application's working
directory is `serverfiles/SAPP_CE`.

## Startup command

AMP launches the server directly rather than calling `run.sh`:

    /usr/bin/wine ./haloceded.exe       -path ./cg       -exec ./cg/init.txt       -port <AMP-assigned-port>

This reproduces the important behavior of the supplied launcher while allowing
AMP to manage the port, process, console, and shutdown command.

## Replace the current GitHub files

Extract this package and replace the existing module files in the repository.
Commit and push the changes to the `main` branch.

Then in ADS:

1. Open **Configuration > Instance Deployment**.
2. Fetch `champeau87/amp-halo-sapp:main`.
3. Refresh the AMP web interface.
4. Create a new **Halo Custom Edition (SAPP)** instance.
5. Enable containerization.
6. Run **Update**.
7. Check File Manager for `SAPP_CE/haloceded.exe` and
   `SAPP_CE/cg/init.txt`.
8. Start the instance.

## Use a fresh instance

Earlier module versions used an incorrect launch path and may have created a
64-bit Wine prefix. This version requires a 32-bit prefix.

The safest route is a new AMP instance. To reuse an old instance:

1. Stop it.
2. Back up `halo-ce/serverfiles/`.
3. Delete only `halo-ce/.wine`.
4. Do not delete `serverfiles`.
5. Run Update and start again.

## First validation

AMP should use this working directory:

    halo-ce/serverfiles/SAPP_CE/

From that directory, it should show a launch command equivalent to:

    /usr/bin/wine ./haloceded.exe -path ./cg -exec ./cg/init.txt -port 2302

In the AMP console, test:

    sv_status

Stopping the instance should send:

    quit

## Configuration locations

Main Halo startup configuration:

    SAPP_CE/cg/init.txt

Per-instance SAPP configuration:

    SAPP_CE/cg/sapp/init.txt

Map cycle and voting:

    SAPP_CE/cg/sapp/mapcycle.txt
    SAPP_CE/cg/sapp/mapvotes.txt

Global SAPP administration and bans:

    SAPP_CE/sapp/admins.txt
    SAPP_CE/sapp/users.txt
    SAPP_CE/sapp/ipbans.txt

The current version intentionally leaves AMP's generated configuration panels
empty. Edit these script-style configuration files through AMP File Manager
until the basic server, console, and shutdown behavior are proven.
