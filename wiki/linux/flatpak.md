# Flatpak

[Flatpak](https://flatpak.org/) is a cross-distribution package manager for
linux systems.

## Setup

The `flatpak` package can be installed by most distribution specific package
managers (e.g. [pacman or yay](/wiki/linux/package_manager.md#arch-linux-pacman-and-yay)).
After installation it is important to add [flathub](https://flathub.org/home),
a source for many Flatpak packages.
The addition of this can be done by running
`flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`.

For browsing and installing Flatpak packages via a graphic user interface the
software management suite called Discover can be used.
It requires the Flatpak backend.

## Usage

### Installing software

When software installation is done with Flatpak it is recommended to pass the
`--user` so the software is not installed system-wide but for the user only.
In practice it looks like the following:
`flatpak install --user <package to install>`.

### Running software

Software installed via Flatpak can be run by typing the full package path into
the command line, for example `com.makemkv.MakeMKV`.
