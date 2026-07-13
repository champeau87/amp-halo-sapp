# AMP Halo Custom Edition + SAPP — PTY Console v7

This release fixes the most likely cause of non-working AMP console input.

## Why v6 could not accept commands

AMP launched:

    /usr/bin/wine ./haloceded.exe ...

Wine could produce output through ordinary pipes, but Halo's old Windows console
did not receive a usable interactive terminal/input handle. As a result:

- commands entered in AMP did not reach Halo;
- AMP's `quit` shutdown command did not reach Halo;
- AMP waited for `App.ExitTimeout=30`;
- AMP then force-stopped the process.

## New launcher

v7 launches through the util-linux `script` command:

    /usr/bin/script -qefc       "/usr/bin/wine ./haloceded.exe -path ./cg -exec ./cg/init.txt -port 2302"       /dev/null

`script` allocates a pseudo-terminal and relays AMP's stdin and stdout through
it. It is not being used to save a transcript; `/dev/null` discards that file.

Options:

- `-q`: suppress script's own start/stop messages
- `-e`: return the child process exit code
- `-f`: flush output promptly
- `-c`: run the supplied command

## Additional managed setting

AMP now exposes:

    Accept Console Commands

This writes:

    console_input 1

to:

    SAPP_CE/cg/sapp/init.txt

## Applying v7

1. Stop the server.
2. Back up:
   - `SAPP_CE/cg/init.txt`
   - `SAPP_CE/cg/sapp/init.txt`
3. Replace the repository files with the v7 files.
4. Push to `main`.
5. Fetch `champeau87/amp-halo-sapp:main` in ADS.
6. Apply the updated template to the Halo instance.
7. Confirm **Accept Console Commands** is enabled.
8. Start the server.

## Validation

In AMP Console, run:

    sv_status

Then run:

    v

Finally click Stop.

Success means:

- commands produce responses;
- Stop shuts down within a few seconds instead of waiting 30 seconds;
- the server process exits without AMP force-killing it.

## Fallback

If startup fails with `/usr/bin/script: No such file or directory`, the AMP Wine
image lacks util-linux's `script` binary. That is unlikely, but in that case the
template will need either an extra container package or a small custom image.
