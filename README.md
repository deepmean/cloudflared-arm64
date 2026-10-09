# cloudflared for Windows ARM64

Unofficial native build of [cloudflare/cloudflared](https://github.com/cloudflare/cloudflared)
for Windows on ARM64, compiled with GitHub Actions.

## Why this exists

Cloudflare does not publish a Windows ARM64 build. As of the `2026.10.0`
release, the Windows assets are only:

| Asset | Windows ARM64? |
|---|---|
| `cloudflared-windows-amd64.exe` | no |
| `cloudflared-windows-386.exe` | no |
| `cloudflared-windows-arm64.exe` | **not published** |

ARM64 Windows builds do exist for Linux (`cloudflared-linux-arm64`) and macOS
(`cloudflared-darwin-arm64.tgz`), but not for Windows.

Windows 11 on ARM can run the amd64 build through its x64 compatibility layer,
which usually works. This repository exists for anyone who wants a native
binary, or who hits problems with the emulated one.

## Usage

Download `cloudflared.exe` from the workflow artifacts and place it where your
tool expects it. For the `dsh-pocket` plugin, that is:

```
%USERPROFILE%\.dsh\dsh-pocket\bin\cloudflared.exe
```

Then verify:

```powershell
cloudflared --version
```

## Build it yourself

Trigger the **build-cloudflared-windows-arm64** workflow from the Actions tab,
or push a `v*` tag. The build runs on the GitHub-hosted `windows-11-arm`
runner (Windows 11 Arm64, Visual Studio 2026 image).

The workflow cross-compiles from Go:

```
GOOS=windows GOARCH=arm64 CGO_ENABLED=0 go build
```

Pin the upstream version in `.github/workflows/build-arm64.yml` under
`CLOUDFLARED_REF`.

## Caveats

- This is **not** an official Cloudflare release. It is built from the public
  source with `CGO_ENABLED=0` and `-trimpath`; no Cloudflare signature is applied.
- Windows will show a SmartScreen warning on first run, since the binary is
  unsigned. Verify the SHA256 (published alongside the artifact) before trusting it.
- If you only need a working tunnel, the official amd64 build already runs on
  Windows 11 ARM via the x64 compatibility layer.

## License

The compiled binary inherits [cloudflared](https://github.com/cloudflare/cloudflared)'s
Apache 2.0 license. This repository's workflow files are provided as-is.