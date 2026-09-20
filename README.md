# COPR GUI

A Qt-based graphical user interface for managing [COPR](https://copr.fedorainfracloud.org/) projects.

`copr-gui` provides a desktop interface for working with COPR instances without having to perform common project-management tasks from the command line or web interface.

## Installation
```bash
sudo dnf copr enable qr243vbi/NewProject
sudo dnf install copr-gui
```

## Features

- Connect to a COPR instance using API credentials
- Load and browse COPR projects
- Create and delete projects
- Configure project chroots
- View project information
- Edit project description, instructions, homepage, and contact information
- Configure project options
- Create and manage packages
- Create and manage builds
- Delete packages and builds
- View project data as JSON
- Paginated tables for project data
- Context menus for common actions
- Background execution of API operations using Qt threads
- Qt Quick/QML-based editors for selected project and build configuration

## Screenshots

<img src="https://raw.githubusercontent.com/qr243vbi/qr243vbi_images/refs/heads/copr_gui/copr_gui_overview.png" />

<img src="https://raw.githubusercontent.com/qr243vbi/qr243vbi_images/refs/heads/copr_gui/copr_gui_packages.png" />

<img src="https://raw.githubusercontent.com/qr243vbi/qr243vbi_images/refs/heads/copr_gui/copr_gui_project.png" />

## Requirements

- Python 3
- PyQt6
- `copr` Python package
- Qt 6 with Qt Quick support

The application currently depends on:

```text
PyQt6
copr
```

## Installation

### From source

Clone the repository:

```bash
git clone https://github.com/qr243vbi/copr_gui.git
cd copr_gui
```

Install the package:

```bash
python3 -m pip install .
```

After installation, the `copr-gui` command is available:

```bash
copr-gui
```

For development, an editable installation can be used:

```bash
python3 -m pip install -e .
```

### Fedora RPM

An RPM spec file is included in the repository:

```text
copr_gui.spec
```

The package is built as a noarch Python application and uses the standard Python RPM packaging macros.

The RPM package requires:

- `python3`
- `python3-pyqt6`
- `python3-copr`
- Qt 6 QtDeclarative

## Authentication

When the application starts, it attempts to connect using the COPR configuration available to the `copr` Python client.

If the configuration cannot be used, the application displays a credentials dialog where you can specify:

- COPR URL
- COPR username
- API login
- API token

The default COPR URL is:

```text
https://copr.fedorainfracloud.org
```

Custom COPR instances can also be specified.

## Project management

After connecting to a COPR instance, the application loads the projects belonging to the authenticated user.

Projects are displayed as cards containing information such as:

- Project name
- Description
- Chroots

A project can be opened to access its detailed management interface.

The project context menu provides actions such as:

- Open
- Delete
- View JSON

### Creating a project

A new project can be created by specifying:

- Project name
- One or more chroots

Available chroots can be fetched from the connected COPR instance.

Chroots can either be entered directly as text or edited through the dedicated chroot editor.

## Packages and builds

The project interface provides management of packages and builds.

Depending on the selected section, the application supports operations such as:

- Adding packages
- Editing package information
- Starting builds
- Deleting packages
- Cancelling and deleting builds
- Refreshing project data

Long-running COPR API operations are executed asynchronously so that the graphical interface remains responsive.

## Project options

Project configuration includes COPR options such as:

- Unlisted projects
- Network access
- Automatic pruning
- Development mode
- Module hotfixes
- Delete-after-days
- Bootstrap configuration
- Bootstrap image
- Build isolation
- Fedora branching
- Fedora review
- AppStream
- Runtime dependencies
- Packit forge projects

Project options can be edited through the project window and saved back to COPR.

## Architecture

The application is currently implemented as a Python/Qt application.

The main components are:

```text
copr_gui.py
    │
    ├── PyQt6 user interface
    │
    ├── COPR API client
    │
    ├── Project management
    │
    ├── Package/build management
    │
    ├── QML configuration editors
    │
    └── Qt worker threads
```

The COPR API is accessed through the Python `copr` package.

Network/API operations that may take a noticeable amount of time are moved to `QThread` workers to keep the GUI responsive.

## Project layout

```text
.
├── copr_gui.py
├── copr_gui_source_types/
│   └── *.qml
├── copr_gui.spec
├── setup.py
├── LICENSE
└── README.md
```

### `copr_gui.py`

Contains the main application and Qt widgets, models, dialogs, API workers, and project management functionality.

### `copr_gui_source_types/`

Contains QML components used by the graphical editors.

### `setup.py`

Defines the Python package and installs the `copr-gui` console entry point.

### `copr_gui.spec`

Provides Fedora/RPM packaging metadata.

## Running from the repository

The application can also be started directly from a checkout:

```bash
python3 copr_gui.py
```

Alternatively, install it in editable mode and use:

```bash
copr-gui
```

## Development

Install the project in editable mode:

```bash
python3 -m pip install -e .
```

Run the application:

```bash
copr-gui
```

The project is intentionally kept relatively lightweight and currently consists primarily of Python, PyQt6, and QML.

## Packaging

The project contains packaging metadata for both Python and RPM-based distributions.

Python package metadata is provided by:

```text
setup.py
```

The Fedora RPM package is described by:

```text
copr_gui.spec
```

The package is currently versioned as `0.1.0`.

## License

This project is licensed under the GNU General Public License, version 3 or later.

See [LICENSE](LICENSE) for the complete license text.
