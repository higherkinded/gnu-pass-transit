# gnu-pass-transit

## What is this?

Pass Transit or `gnu-pass-transit`, whatever is the title you're more satisfied
with, is a small and fairly simple script that allows you to package your GNU
Pass storage (didn't test with multiple) and re-encrypt every single password in
it to allow you to, say, share one or sync the password storages with an another
computer of yours without the tedium of doing it manually for each one or the
anxiety of doing it in plaintext.

## How do I use it?

It's fairly simple. First off, you can use it by its relative path:

```
./gnu-pass-transit
```

Secondly, you can send this thing up your `/usr/local/bin` or anywhere on your
path, don't forget to `chmod 644` it afterwards.

To actually use it, you have to have these pre-requisites:

1. `shred` must be present on your `PATH`. You can disable shredding by setting
 `__DO_SHRED` in the file to `False`, though that is to say, user discretion is
 advised.

2. GNU `pass`, obviously.

3. If you're on a recieving side, you just have to have a GnuPG pair. Public key
 is, as per usual, given to the sender. Sender re-encrypts the passwords with it
 and gives you a gpg-ed tarball that contains the passwords they have sent.

4. If you're on a sending side, you get the public key, probably provide a list
 of passwords you want exported, and send a tarball to the recipient.

### Exporting stuff

So, actual export of the storage is done this way:

```
./gnu-pass-transit -r their-email@foo.bar
```

If you want to select the passwords before actually doing it, you can do this:

```
./gnu-pass-transit -r their-email@foo.bar -e passname1 passname2
```

Once the export is done, the resulting encrypted tarball will be placed at
`$HOME/.local/share/pass_export`, the `*.tar.gz.gpg` one, yep.

### Importing stuff

Just do it like this:

```
./gnu-pass-transit -r my-key-email@bar.baz -i yourtarballhere.tar.gz.gpg
```

If a collision is detected, you'll be given an option to decline the overwrite
or to go through with it, it's your call.
