Ansible role: uv
================

Install and configure uv python package and project manager.

Role Variables
--------------

```
ENTRY POINT: *main* - Install and configure uv python package and project manager

Options (= indicates it is required):

- uv_archive_extension  File extension for the uv package archive
          default: tar.gz
          type: str

- uv_bin_dir  Directory for the uv binaries
          default: /opt/uv
          type: str

- uv_cache_prune_randomized_delay  Delay the uv cache prune timer by
                                    a random time up to this value or
                                    empty string for no delay
          default: 6h
          type: str

- uv_cache_prune_time  How often to prune the uv cache, accepts a
                        systemd time, see
                        https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html,
                        or "never"
          default: never
          type: str

- uv_checksum_type  The uv package checksum type
          default: sha256
          type: str

- uv_clean_src_dir  Remove old downloaded archive files from uv src
                     directory
          default: true
          type: bool

- uv_cli_tools  List of CLI tool binaries that should have symlinks
                 created in /usr/local/bin/
          default: [uv, uvx]
          type: list

- uv_github_org  Name of organisation for uv github repository
          default: astral-sh
          type: str

- uv_github_repo  Name of uv github repository
          default: uv
          type: str

- uv_github_token  Optional bearer token to use to authenticate with
                    api.github.com
          default: ''
          type: str

- uv_install  If true, install uv
          default: true
          type: bool

- uv_src_dir  Directory for the downloaded uv src archive
          default: /opt/uv/src
          type: str

- uv_src_files  List of files to extract from the source archive
          default: [uv, uvx]
          elements: str
          type: list

- uv_strip_components  Strip NUMBER leading components from file
                        names on extraction
          default: 1
          type: int

- uv_tool_upgrade_randomized_delay  Delay the uv tool upgrade timer
                                     by a random time up to this value
                                     or empty string for no delay
          default: 6h
          type: str

- uv_tool_upgrade_time  How often to upgrade uv tools, accepts a
                         systemd time, see
                         https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html,
                         or "never"
          default: never
          type: str

- uv_update_randomized_delay  Delay the uv update timer by a random
                               time up to this value or empty string
                               for no delay
          default: 6h
          type: str

- uv_update_time  How often to update uv, accepts a systemd time, see
                   https://www.freedesktop.org/software/systemd/man/latest/systemd.time.html,
                   or "never"
          default: daily
          type: str

- uv_users  List of user accounts to install uv for
          default: []
          elements: dict
          type: list
          options:

          - tools  Python tools to install with uv
            default: null
            elements: dict
            type: list
            options:

            - install  Tool install arguments
              default: null
              type: str

            = name  Tool name
              type: str

            - state  Tool state
              choices: [present, absent]
              default: present
              type: str

          = username  Name of the user account
            type: str

          - venvs  Python virtual environments to install with uv
            default: null
            elements: dict
            type: list
            options:

            - lockfile  Python dependencies to install in virtual
                         environment in requirements.txt or
                         pylock.toml format with uv pip sync
              default: null
              type: str

            = path  Virtual environment path
              type: str

            - pip_install_args  Extra arguments to provide to uv pip
                                 install
              default: null
              type: str

            - pip_sync_args  Extra arguments to provide to uv pip
                              sync
              default: null
              type: str

            - requirements  Python dependencies to install in virtual
                             environment in requirements.txt format
                             with uv pip install
              default: null
              type: str

            - state  Virtual environment state
              choices: [present, absent]
              default: present
              type: str

- uv_version  Version to install (use "latest" for the latest
               version)
          default: latest
          type: str
```

Installation
------------

This role can either be installed manually with the ansible-galaxy CLI tool:

    ansible-galaxy install git+https://github.com/wandansible/uv,main,wandansible.uv

Or, by adding the following to `requirements.yml`:

    - name: wandansible.uv
      src: https://github.com/wandansible/uv

Roles listed in `requirements.yml` can be installed with the following ansible-galaxy command:

    ansible-galaxy install -r requirements.yml

Example Playbook
----------------

    - hosts: all
      roles:
         - role: wandansible.uv
           become: true
           vars:
             uv_users:
               - username: user
                 tools:
                   - name: ansible
                     install: --with-executables-from ansible-core,ansible-lint ansible
                   - name: yt-dlp
                     install: yt-dlp[default]
                   - name: ruff
                 venvs:
                   - path: ~/.local/share/uv/venvs/test
                     requirements: |
                       pynvim
