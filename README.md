# PDX Ruby Brigade Credentials

We're using [Mozilla's sops](https://github.com/mozilla/sops) to encrypt the credentials.

## Getting Access

Issue a pull request and add your __PUBLIC__ GPG fingerprint to the `# GPG Fingerprints` section of
`.sops.yaml`. You can find your public fingerprint by running

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

In this example, it's `820A5BE1B38C55E51C2CA163FA065807F03AB48B`.

If you only need access to certain credentials, please state so in the PR.

## Adding and removing keys

For more info, see [sops#adding-and-removing-keys](https://github.com/mozilla/sops#adding-and-removing-keys)
but the commands are:

This would add Jesse's key only for the `twitter.yaml` file.
```
sops -r --add-pgp 820A5BE1B38C55E51C2CA163FA065807F03AB48B twitter.yaml
```

This would remove Jesse's key only for the `twitter.yaml` file.
```
sops -r --rm-pgp 820A5BE1B38C55E51C2CA163FA065807F03AB48B twitter.yaml
```

## Key rotation

Make sure to add the `-r` flag so the data key will be rotated, especially when removing a key.
This will prevent owners of the removed key from having add access in the past.
