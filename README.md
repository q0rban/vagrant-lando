# Lando in Vagrant

## _WHY?!_

_WHY_ you might ask? On Mac, Docker performance can be particularly atrocious.
Using Lando inside Vagrant allows docker to use Linux's native file system,
resulting in significant performance improvements over Docker for Mac.

## Install

If this is your first time you are using Vagrant, you will need:

* Git, to be able to clone this repository. Mac users should update git using
  [Homebrew](http://brew.sh), but it's not required. Windows users can use the
  excellent [Babun](http://babun.github.io) or get Git directly from
  [git-scm.com](https://git-scm.com).
* [VirtualBox](https://www.virtualbox.org), used to run virtual machines. Make
  sure you are using the latest version from the website.
* [Vagrant](https://www.vagrantup.com), the CLI tool to manage virtual
  machines easily. Make sure you are using the latest version from the website.
  
Once you have the above, you can either `git clone` this repo, fork it, or
if you have an existing project, you can copy the files from this repo into an
existing project. For a one-liner using cURL, replace `/path/to/my-project` with
the path to the project you'd like to download these files to:

```
curl --fail --silent --show-error --location \
  https://codeload.github.com/q0rban/vagrant-lando/tar.gz/master \
  | tar -xvz -C /path/to/my-project --strip-components 1
```

## Customize

Once you have the files, copy the `dist/Vagrant.yml` file to
`Vagrant.yml`:

```
cp dist/Vagrant.yml Vagrant.yml
```

Then, read through that file and make modifications as you see fit for the needs
of your project.

### File syncing

The most important change you can make in here is setting up your preferred file
syncing type. If you are on Mac, it is recommended you set `sync` to `'nfs'`. If
you are on Windows, `'smb'` is recommended. For the fastest performance, set
`sync` to `'rsync'`, but you'll have to call `vagrant rsync` every so often to
get files back and forth between the host and the vm.
