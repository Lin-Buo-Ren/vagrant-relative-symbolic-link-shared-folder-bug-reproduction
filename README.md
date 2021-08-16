# Vagrant relative symbolic link shared folder bug reproduction

Vagrant errors when a relative symbolic link points to an in-project directory is in a Rsync-based shared folder

## Reproduce steps

1. Clone this project's repository under a system supporting in-repo symbolic link(Windows may need additional configuration or permission).
1. Launch a text terminal and change the working directory to the project directory.
1. Run the `vagrant up` command

## Current behavior

Vagrant triggeres a "There was an error when attempting to rsync a synced folder." non-fatal error: [with-complex-pwd-path.vagrant.out](with-complex-pwd-path.vagrant.out) (debug log: [without-complex-pwd-path.vagrant.debug.out](without-complex-pwd-path.vagrant.debug.out))

Note that when a path containing CJK characters is used, [a Encoding::CompatibilityError fatal error is raised instead](with-complex-pwd-path.vagrant.out)(debug log: [with-complex-pwd-path.vagrant.debug.out](with-complex-pwd-path.vagrant.debug.out)), which is a separate issue that is filed separately(https://github.com/hashicorp/vagrant/issues/12494).  To test this configuration in particular, clone the project's repository under `~/中文測試路徑` in reproduce step 1

## Expected behavior

Rsync-emulated shared folder successfully created, symbolic link references to its intended destination without issues.

## Workarounds

Avoid using the Rsync-emulated shared folder (may require using a different box that supports VirtualBox shared folders)

For the centos/7 box you can use [the alternative created by the Bento project](https://app.vagrantup.com/bento/boxes/centos-7)

## Reference

* [Vagrant errors when a relative symbolic link points to an in-project directory is in a Rsync-based shared folder · Issue #12493 · hashicorp/vagrant](https://github.com/hashicorp/vagrant/issues/12493)
* [Vagrant::Errors::RSyncError triggers Encoding::CompatibilityError fatal error when project-dir contains non-ASCII characters · Issue #12494 · hashicorp/vagrant](https://github.com/hashicorp/vagrant/issues/12494)
