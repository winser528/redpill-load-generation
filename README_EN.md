# Syno Generation

[中文说明](README.md "English")

## Usage

1. Edit `<platform>_user_config.json` that matches your <platform_version> according [redpill-load](https://github.com/RedPill-TTG/redpill-load) and place it in the same folder as redpill_tool_chain.sh
2. Build the image for the platform and version you want:
   `./redpill_tool_chain.sh build <platform_version>`
3. Run the image for the platform and version you want:
   `./redpill_tool_chain.sh auto <platform_version>`

You can always use `./redpill_tool_chain.sh run <platform_version>` to get a bash prompt, modify whatever you want and finaly execute `make -C /opt/build_all` to build the boot loader image.
After step 3. the redpill load image should be build and can be found in the host folder "images".

Note1: run `./redpill_tool_chain.sh` to get the list of supported ids for the <platform_version> parameter.
Note2: if `docker.use_local_rp_load` is set to `true`, the auto action will not pull latest redpill-load sources.
Note3: Please do not ask to add <platform_version> with configurations for other redpill-load repositories.

Feel free to modify any values in `global_config.json` that suite your needs!

### See Help text

```txt
./redpill_tool_chain.sh
Usage: ./redpill_tool_chain.sh <action> <platform version>

Actions: build, auto, run, clean, add, del, sn, pat

- build:    Build the toolchain image for the specified platform version.

- auto:     Starts the toolchain container using the previosuly build toolchain image for the specified platform.
            Updates redpill sources and builds the bootloader image automaticaly. Will end the container once done.

- run:      Starts the toolchain container using the previously built toolchain image for the specified platform.
            Interactive Bash terminal.

- clean:    Removes old (=dangling) images and the build cache for a platform version.
            Use ‘all’ as platform version to remove images and build caches for all platform versions.

- add:      To install extension you need to know its index file location and nothing more.
            eg: add 'https://example.com/some-extension/rpext-index.json'

- del:      To remove an already installed extension you need to know its ID.
            eg: del 'example_dev.some_extension'

- sn:       Generates a serial number and mac address for the following platforms
            DS3615xs DS3617xs DS916+ DS918+ DS920+ DS3622xs+ FS6400 DVA3219 DVA3221 DS1621+
            eg: sn ds920p

- pat:      For decoding PAT file. 

Available platform versions:
---------------------
ds918p-6.2.4-25556
ds918p-7.0.1-42218
ds918p-7.1.0-42661
ds920p-7.0.1-42218
ds920p-7.1.0-42661
ds923p-7.1.1-42962
ds1621p-7.0.1-42218
ds1621p-7.1.0-42661
ds2422p-7.0.1-42218
ds3615xs-6.2.4-25556
ds3615xs-7.0.1-42218
ds3615xs-7.1.0-42661
ds3617xs-7.0.1-42218
ds3617xs-7.1.0-42661
ds3622xsp-7.0.1-42218
ds3622xsp-7.1.0-42661
dva3221-7.0.1-42218
dva3221-7.1.0-42661

Custom Extensions:
---------------------
jumkey.acpid2
thethorgroup.boot-wait
thethorgroup.virtio

Check global_settings.json for settings.
```
