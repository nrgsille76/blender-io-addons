
VERSION := $(shell grep '^version =' source/blender_manifest.toml | sed 's/[a-z]* = "\(.*\)"/\1/')
EXTENSION_ZIP := io_scene_3ds-${VERSION}.zip

@echo "-------------------------------------------------"
@printf "\033[95mCreating extension ZIP file (version ${VERSION})\033[0m\n"
@echo "-------------------------------------------------"
blender --command extension validate source/
blender --command extension build --source-dir source/ --output-filepath ${PWD}/dist/${EXTENSION_ZIP}
@printf "Created: \033[92m${PWD}/dist/${EXTENSION_ZIP}\033[0m\n"
@echo "-------------------------------------------------"

