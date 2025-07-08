# Moving GPG keys to a new machine

Sometimes, you get a new machine and you need to move your GPG key to that new computer. This is how to do it.

GPG keys come in two parts:

- Public Keys
- Private Keys (or "Secret" keys).

When exporting GPG keys from GPG, the exported "Secret" key file contains both the Public and Private Key, so we only
need to move one file!

In order to move them, we need to do the following steps:

1. Get the Key *name*
2. Export to a secret key file
3. Move the file to your new computer
4. Import the key pair from the file
5. Trust the key pair on your new computer.

## Get the key name

```bash
gpg --list-secret-keys
```

You'll get an output that looks like:

```
/home/matt/.gnupg/pubring.kbx
-----------------------------
sec   ed25519 2024-08-28 [SC]
      593E00FDAAE3C390075AB63948E2FABDE0B3B2DE
uid           [ultimate] Test <test@example.com>
ssb   cv25519 2024-08-28 [E]
```

That `593E00FDAAE3C390075AB63948E2FABDE0B3B2DE`-looking thing is your key name.

## Export to a secret key file

We're going to export to an ASCII armored file:

```bash
gpg --armor --export-secret-key <key_name> > secret.asc

# e.g.
gpg --armor --export-secret-key "593E00FDAAE3C390075AB63948E2FABDE0B3B2DE" > secret.asc
```

This creates a `secret.asc` file containing your GPG public-private keypair.

## Move the file to your new computer

USB stick?

Ideally, something "offline" / not via the internet.

## Import the key pair from the file

Once you've got the file on your new computer, run:

```bash
gpg --import secret.asc
```

## Trust the key pair on your new computer

To trust the key pair, we need to edit the key to open a new gpg key management prompt for the key.

```bash
gpg --edit-key <key_name>
# e.g. 
gpg --edit-key "593E00FDAAE3C390075AB63948E2FABDE0B3B2DE"
```

Using the gpg prompt, trust the key:

```
gpg> trust
```

This will prompt you as follows:

```
Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

  1 = I don't know or won't say
  2 = I do NOT trust
  3 = I trust marginally
  4 = I trust fully
  5 = I trust ultimately
  m = back to the main menu

Your decision?
```

assuming it's your GPG key, select `5` for ultimate trust, confirm with `y`.

When you are returned to the gpg prompt, simply do:

```bash
gpg> save
```

---

And you're done! Enjoying using your GPG keys on your new machine.

You can see that both Public and Private keys exist and are trusted, by running the following commands:

```bash
gpg --list-keys
gpg --list-secret-keys
```

~ Harmelodic / Matt Smith
