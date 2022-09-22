# AlcanceLibre.org AppCenter

This is a fork of on 3.0.1 of AppCenter from elementaryOS. It's customized to work with ALDOS (a Fedora fork without SystemD) removing as much as possible elementaryOS specific features.

The main goal of this fork is to develop a fully distribution-neutral Software Center.

![AppCenter Screenshot](data/aldos-appcenter.png?raw=true)

## Reasons for this fork

* Upstream only cares about elementaryOS and have no plans to support other Linux distributions beside the forgotten initial promise to make a distribution-neutral software center
* Upstream direction goes to become flatpak-only, which I think is a bad idea
* The best alternative (gnome-software) requires SystemD for system updates and complete functionality
* The second best alternative (discovery) requires KF5 and KDE Plasma components and offers installation for non-desktop-neutral extensions, plugins and themes.

## Todo

* Remove any remote connections to API server from elementaryOS (currently number of packages shown in carousel and banner are set to 0)
* Use only the local data stored at /usr/share/app-info/xmls for featured and popular applications
* Easily enable or disable use of native or flatpak packages
* Remove the payments support code
* Remove any other elementaryOS specific feature
* Update code. Currently only builds with appstream 0.12.x. Does not build with appstream >= 0.15.

## Building, Testing, and Installation

You'll need the following dependencies:

* appstream >= 0.10.0, <= 0.13
* appstream-vala >= 0.10.0, <= 0.13
* desktop-file-utils
* gee-0.8 >= 0.20.1
* gettext
* glib-2.0 >= 2.56.0
* gobject-introspection-1.0 >= 1.56.0
* granite >= 5.0
* gthread-2.0
* gtk+-3.0 >= 3.12
* intltool
* json-glib-1.0
* libappstream-glib
* libsoup-2.4 >= 2.62.0
* meson
* packagekit-glib2
* vala >= 0.40

Optional:

* libunity >= 4.0.0

Run `meson build` to configure the build environment. Change to the build directory and run `ninja` to build

    meson build --prefix=/usr -D payments=false -D curated=false -D sharing=false -D libunity=false
    cd build
    ninja

To install, use `ninja install`, then execute with `org.alcancelibre.appcenter`

    sudo ninja install
    org.alcancelibre.appcenter

## Debugging

See debug messages:
As specified in the [GLib documentation](https://developer.gnome.org/glib/stable/glib-running.html)

    G_MESSAGES_DEBUG=all org.alcancelibre.appcenter

Show restart required messaging:

    sudo touch /var/run/reboot-required

Hide restart required messaging:

    sudo rm /var/run/reboot-required

Fake updates with the `-f` flag followed by PackageKit package name, **not** appstream id:

    org.alcancelibre.appcenter -f inkscape

Load and preview a local AppStream XML metadata file, your local metadata will show up in the featured banner and will also be searchable. Metadata loaded this way will have a `(local)` suffix in it's name.

    org.alcancelibre.appcenter --load-local /path/to/file.appdata.xml

