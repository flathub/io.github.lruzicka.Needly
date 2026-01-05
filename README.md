# Needly Flatpak

This repository contains the Flatpak manifest and related files for packaging Needly, the openQA needle editor, for distribution on Flathub.

## About Needly

Needly is a Python-based openQA needle editor that creates and modifies needles for openQA tests. It features:
- WYSIWYG needle editor
- Screenshot handling
- Virtual machine screenshot capture via libvirt
- Support for multiple areas in a single needle

## Files in this Repository

- `io.github.lruzicka.Needly.yaml` - Main Flatpak manifest file
- `python3-tkinter.yaml` - Module for building Tcl/Tk libraries required by Tkinter
- `io.github.lruzicka.Needly.desktop` - Desktop entry file
- `io.github.lruzicka.Needly.metainfo.xml` - AppStream metadata file
- `needly/` - Clone of the Needly source repository (for reference)

## Building from Source

The manifest is configured to build from the latest sources on the `main` branch of the Needly repository. This ensures that Flathub builds will always use the most recent version.

### Build Configuration

The manifest includes:
- **Runtime**: org.freedesktop.Platform 24.08
- **SDK**: org.freedesktop.Sdk 24.08
- **Command**: needly

### Dependencies Built

1. **Tcl/Tk** (8.6.14) - For Tkinter GUI support
2. **libtirpc** (1.3.4) - RPC library for libvirt
3. **rpcsvc-proto** (1.4.4) - RPC protocol definitions
4. **libvirt** (10.9.0) - Virtualization API for VM screenshot capture
5. **Python dependencies**:
   - Pillow (11.0.0) - Image processing
   - libvirt-python (11.0.0) - Python bindings for libvirt

### Permissions

The application requests the following permissions:
- `--share=ipc` - Inter-process communication
- `--socket=x11` - X11 display access
- `--socket=wayland` - Wayland display access
- `--filesystem=home` - Read/write access to home directory for needle files
- `--filesystem=xdg-run/gvfs` - Access to GVFS mounts
- `--filesystem=xdg-run/libvirt:ro` - Read-only access to libvirt user session
- `--filesystem=/run/libvirt:ro` - Read-only access to libvirt system session
- `--system-talk-name=org.libvirt.libvirtd` - D-Bus access to libvirt daemon

## Testing Locally

To test the Flatpak build locally:

```bash
# Install flatpak-builder if not already installed
sudo dnf install flatpak-builder  # Fedora
sudo apt install flatpak-builder   # Debian/Ubuntu

# Build and install locally
flatpak-builder --user --install --force-clean build-dir io.github.lruzicka.Needly.yaml

# Run the application
flatpak run io.github.lruzicka.Needly
```

## Submitting to Flathub

To submit this to Flathub:

1. Fork the Flathub repository at https://github.com/flathub/flathub
2. Follow the Flathub submission guidelines at https://docs.flathub.org/docs/for-app-authors/submission/
3. Create a new repository under the Flathub organization named `io.github.lruzicka.Needly`
4. Push these files to that repository
5. Submit a pull request to the main Flathub repository

Note: Since Needly already exists on Flathub (https://github.com/flathub/io.github.lruzicka.Needly), you can update the existing repository with this new manifest instead.

## Updating Dependencies

When Needly updates its dependencies, you'll need to:

1. Update the version numbers in `pyproject.toml` references
2. Update the SHA256 hashes for the Python packages
3. Update the libvirt version if needed
4. Update the release information in `io.github.lruzicka.Needly.metainfo.xml`

To get SHA256 hashes for Python packages:
```bash
# For Pillow
curl -s https://pypi.org/pypi/Pillow/11.0.0/json | jq -r '.urls[] | select(.packagetype=="sdist") | .digests.sha256'

# For libvirt-python
curl -s https://pypi.org/pypi/libvirt-python/11.0.0/json | jq -r '.urls[] | select(.packagetype=="sdist") | .digests.sha256'
```

## Automatic Source Updates

The manifest uses `branch: main` for the Needly source, which means:
- Flathub's automatic build system will pull the latest changes from the main branch
- Each build will use the most recent version of the source code
- No manual updates are needed for new Needly releases (only for dependency updates)

## License

The Flatpak manifest and related packaging files are licensed under the same license as Needly (GPL-3.0).

## Links

- **Needly Repository**: https://github.com/lruzicka/needly
- **Flathub Page**: https://flathub.org/apps/io.github.lruzicka.Needly
- **Flathub Repository**: https://github.com/flathub/io.github.lruzicka.Needly
