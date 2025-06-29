> [!IMPORTANT]
> I am spread too thin between all of my projects, so if you can support my efforts or wish to contribute - please contact me.
>
> My username here is almost the same on most popular online social platforms.
>
> I am open to business proposals as well. CV at LinkedIn.

# Bash Utility Functions Library

A collection of pre-made commands reducing cognitive load on the user.

Can be useful for active `rpm-ostree`, `podman`, `distrobox` & `adb` users.

> [!NOTE]
> Intended for `source` (or `.`).

> [!WARNING]
> Use this code at your own risk, it was not well-tested!
>
> I am a solo developer with little resources so I can't guarantee smooth operation.
>
> I am writing these scripts to be stable to the best of my abilities and knowledge.

# Installation

## Define installation function for current session

```sh
bash_utils_install_clone() {
	local install_root="${1:?"Installation directory is required"}"
	local git_remote="${2:?"Git remote is required"}"
	local author_name="$(echo "${git_remote}" | sed -E 's/.*[:/]([^/]+)\/.*/\1/')"
	local project_name="$(basename "${git_remote}" .git)"
	local install_dir="${install_root}/${author_name}/${project_name}"
	local install_dir_rel="\$(realpath \$(dirname \"\${BASH_SOURCE[0]}\"))/${author_name}/${project_name}"
	mkdir -p "${install_dir}"
	git clone "${git_remote}" "${install_dir}"
	# This will generate a recursive importer script that goes exactly 2 levels deep in the file tree - to author and then to repo itself
	echo -e "for func_lib in ${install_dir_rel}/*.sh; do
		source \"\${func_lib}\"
	done" >> "${install_root}/source.import.${author_name}.${project_name}.sh"
}
```

> [!TIP]
> To keep your system de-cluttered, install scripts to a location shared between users.

> [!CAUTION]
> Be careful with elevated privileges execution and important system files editing!

## Pick scope path

| User scope    | For login sessions | For interactive sessions |
|---------------|--------------------|--------------------------|
| System-wide   | `/etc/profile.d/`  | `/etc/bashrc.d/`         |
| User-specific | `~/.profile.d/`    | `~/.bashrc.d/`           |

Edit & add `for f in $scope/*.sh; do source $f; done` to the default sourced file (e.g. `/etc/bashrc`).

## Install

```sh
sudo bash_utils_install_clone "${scope_path}" git@github.com:tsilvs/bash_utils.git
```

# Plans

+ [ ] Proper i18n with separate locale files
+ [ ] Rewrite with any `bash` scripting library (e.g. `aks/bash-lib`?) or pre-processor (e.g. `TypeShell`?) for better stability, reliability and maintainability
+ [ ] UI with `yad`
+ [ ] Proper packaging with `.rpm`, `.deb`, `pkgbuild`, `flatpak`, `bpkg`, `podman` image or any other suitable packaging tool
+ [ ] Programming language rewrite?
	+ [ ] Interpreted (Ruby, TypeScript, Python)?
	+ [ ] Compiled (Zig, Rust, Crystal, Go, Kotlin, Haskell)?
	+ [ ] any other that's well integrated with Linux ecosystem?