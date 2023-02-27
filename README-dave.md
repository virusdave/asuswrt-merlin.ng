# Building instructions

This repo requires a case-sensitive filesystem, since there are case collisions
in the linux kernel tree.  Thus, we've moved the repo into this case-sensitive
volume image.
Mount the image to /Volumes/CaseSensitiveFS, then launch the build container via:

```
docker run -it --rm  \
  -v "/Volumes/CaseSensitiveFS/build:/build" \
  gnuton/asuswrt-merlin-toolchains-docker:latest \
  /bin/bash
```

## RT-AX88U specific stuff

For the rt-ax88u, we need additional modules built for encrypted drive support.
Sadly, just modifying the `config.6a` kernel config file seemed insufficient,
as the make process kept overwriting it.

However, one can build the image and explicitly specify additional kernel
config options at build time as follows:

(in the `release/src-rt-5*` directory)

```
make rt-ax88u \
  EXTRA_KERNEL_CONFIGS="CRYPTO_USER=m CRYPTO_USER_API=m CRYPTO_USER_API_HASH=m CRYPTO_USER_API_SKCIPHER=m CRYPTO_USER_API_RNG=m CYRPTO_USER_API_AEAD=m"
```

(Note the missing `CONFIG_` prefix on the options themselves!!)

This forces the various kernal config options to be appended to
the `.config` file used in the compiling the kernel.


## Reduce disk space
Use sparse checkout to only see the files for the router model you use:

```
$ cat .git/info/sparse-checkout

/*
!/*/
/release/
!/release/*/
/release/src/
/release/src-rt/
/release/src-rt-5.02axhnd/
/release/src-rt-6.x.4708/
```

