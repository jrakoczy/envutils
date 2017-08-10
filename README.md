# Envutils

A set of tools that allow to conveniently load and test user environment
files on NixOS.

## Rationale

NixOS currently doesn't provide a convenient way to manage user dot-files
(although this may be a subject to change as [a more robust form of this
feature is being actively developed](https://github.com/NixOS/nixpkgs/pull/9250)).

I wanted a declarative solution that would allow me to exploit all benefits
of the Nix package manager while still providing convenient means to test a
configuration file without a need to rebuild everything.

Envutils takes a simplified approach. I've implemented it to get more
experience with the system and its package manager. It suits my needs, but
you may want to consider [other options](https://github.com/rycee/home-manager)
if you need something more than mere dot-files management.

## Usage

The main idea is pretty simple — you keep your immutable user
environment files/dot-files in the Nix store and you sym-link them to the
user's home directory.

Envutils should be used in conjunction with [\*-environment packages in aux-nixpkgs](https://github.com/jrakoczy/aux-nixpkgs)
or something equivalent, since you want get all the dot-files to the Nix
store in the first place. Then a given user can install whichever
environment package they choose by running:

```bash
nix-env -i <env-name>-environment
```

However, this doesn't mean that the environment is loaded yet.

For more information on how to add a custom nixpkgs repository, please take a lookat [the aux-nixpkgs README](https://github.com/jrakoczy/aux-nixpkgs/blob/master/README.md).

### Loading

Once any of `*-environment` packages is installed, simply run:

```bash
envload
```

And that's it. All files are in the user's home directory.

*Please bear in mind, that this command will delete previously loaded
files and overwrite any manually added or modified ones.*

### Deleting

`envdel` will remove any files loaded to the user's home directory by a
previous execution of `envload`. If some directories get empty in the
process, they will be removed.

### Testing

`envtest` cleverly sym-links files or directories to your user's home. For
more information, please run:

```bash
envtest  --h
```


