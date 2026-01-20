# ZMK Charybdis Config

This repo contains ZMK configuration for a BastardKB Charybdis (nice!nano, split).

## Build Prereqs

- Podman
- The `zmk-dev-arm` container image

## Build From a Fresh Clone

From the repo root on the host:

```sh
podman run --rm -it \
  -v "$PWD:/work:z" \
  -w /work \
  docker.io/zmkfirmware/zmk-dev-arm:stable \
  bash
```

Inside the container:

```sh
west init -l config
west update
west zephyr-export
```

Build each half:

```sh
west build -p -s zmk/app -b nice_nano -d build/left -- -DZMK_CONFIG=/work/config -DSHIELD=charybdis_left
west build -p -s zmk/app -b nice_nano -d build/right -- -DZMK_CONFIG=/work/config -DSHIELD=charybdis_right
```

## Output Files

After building, the UF2 files are:

- `build/left/zephyr/zmk.uf2`
- `build/right/zephyr/zmk.uf2`

## Flashing

1) Put the half into bootloader (double-tap reset).
2) A drive named `NICENANO` appears.
3) Copy the correct `zmk.uf2` to it.

Repeat for the other half.
