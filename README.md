# PDX Ruby Brigade Credentials

We're using [Mozilla's sops](https://github.com/mozilla/sops) to encrypt the credentials.

## Prerequisites
If you do not already have a GPG key, install `gpg2` and create a 4096 RSA key.

Optionally, you can upload the __PUBLIC__ key to a keyserver; for example [MIT's key server](https://pgp.mit.edu). (It does not matter what keyserver, as long as the keyserver is in the keyserver pool). Then at the next PDX Ruby Meetup, bring a valid ID and several paper copies of your __PUBLIC__ GPG key fingerprint to exchange; so that other GPG key holders can later digitally sign your key and you theirs.

Note, there are some file storage changes between versions 1 and 2 of `gpg`; recent versions of `sops` fixed compatibility issues (August 2017, version 2.0.10). However, there are still GitHub Issues and links on the web citing problems and some common error messages can be a little confusing referencing files that `gpg2` does not create.

__NB: be sure to update your environmental vars__ (in for example a `.bashrc, .bash_profile, or .zshrc`) with
```
GPG_TTY=$(tty)
export GPG_TTY
```
otherwise `sops` will not be able to find your secret key for decryption/encryption.

`sops` itself can be installed via `homebrew` on macOSX; `rpms, debs, and exes` are available on the [sops repo](https://github.com/mozilla/sops/releases).

## Getting Access
Issue a pull request and add your name, github ID, and __PUBLIC__ GPG key fingerprint to the `fingerprints.txt`
. You can find your public fingerprint by running

```
> gpg --list-keys

/Users/jesse/.gnupg/pubring.kbx
-------------------------------
pub   rsa4096 2014-10-27 [SC]
      820A5BE1B38C55E51C2CA163FA065807F03AB48B
uid           [ unknown] Jesse Cooke <jesse@jc00ke.com>
uid           [ unknown] keybase.io/jc00ke <jc00ke@keybase.io>
sub   rsa4096 2014-10-27 [E]
```

In this example, the fingerprint is `820A5BE1B38C55E51C2CA163FA065807F03AB48B`, so you would append
`Jesse Cooke - @jc00ke:  820A5BE1B38C55E51C2CA163FA065807F03AB48B` to `fingerprints.txt`.

If you only need access to certain credentials, please state so in the PR.

## Decrypting files
__SEE NOTE ABOUT ENVIRONMENTAL VARIABLES__

Once your pull request has been accepted and the repo updated, simply

`sops -d twitter.yaml`

NB: always remember to tweet responsibly :)

## Adding and removing keys

For more info, see [sops#adding-and-removing-keys](https://github.com/mozilla/sops#adding-and-removing-keys)
but the commands are:

This would add Jesse's key only for the `twitter.yaml` file.
```
sops -r -i --add-pgp 820A5BE1B38C55E51C2CA163FA065807F03AB48B twitter.yaml
```

Note: If the public key being added is on a keyserver, it'll get pulled in
automatically. If not, it needs to be in the local keyring.

This would remove Jesse's key only for the `twitter.yaml` file.
```
sops -r -i --rm-pgp 820A5BE1B38C55E51C2CA163FA065807F03AB48B twitter.yaml
```

## Key rotation

Make sure to add the `-r` flag so the data key will be rotated, especially when removing a key.
This will prevent owners of the removed key from having add access in the past.
