# AMP Halo Custom Edition + SAPP

This repository contains a custom AMP Generic Module template for running the
Halo Custom Edition dedicated server with SAPP under Wine on Linux.

## What this template does

It launches:

    /usr/bin/wine ./haloceded.exe -path . -port <AMP assigned GamePort>

The template does not download Halo or SAPP files. Upload files you are legally
permitted to use into the instance base directory.

## Required base-directory layout

    haloceded.exe
    sapp.dll
    strings.dll
    init.txt
    maps/

Use the SAPP-supplied haloceded.exe that matches the SAPP package.

## Install/update the template

1. Replace the files in the GitHub repository with these files and push them.
2. In ADS, open Configuration > Instance Deployment.
3. Fetch the `champeau87/amp-halo-sapp:main` configuration repository again.
4. Refresh the browser.
5. For the cleanest test, create a new instance and enable containerization.
6. Upload the required server files into `halo-ce/serverfiles/`.

The template recommends:

    cubecoders/ampbase:wine-stable

## Existing instance warning

The old template created a 64-bit Wine prefix. This replacement uses a 32-bit
prefix. Changing WINEARCH does not convert an existing prefix.

Back up `serverfiles`, then either:

- create a fresh instance; or
- stop the instance and remove the instance's `halo-ce/.wine` directory before
  starting it again.

Do not remove `serverfiles`.

## First test

The expected launch command is:

    /usr/bin/wine ./haloceded.exe -path . -port 2302

A successful test should:

1. keep the process running;
2. show the Halo/SAPP console output;
3. respond to `sv_status`;
4. stop after AMP sends `quit`.

## If the process runs but console input does not work

Try Wine's terminal console frontend by changing these two KVP lines:

    App.ExecutableLinux=/usr/bin/wineconsole
    App.LinuxCommandLineArgs=--backend=curses ./haloceded.exe

Keep `App.CommandLineArgs` unchanged. This is a fallback only; test direct Wine
first.
