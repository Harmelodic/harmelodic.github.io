# Git with SSH

When first starting out with Git and remote repositories, most people connect with HTTPS URLs, and thus use
Username/Password to authenticate.

However, this has a couple of drawbacks:

- Typing in your Username/Password every time can be really annoying and runs the risk of you accidentally leaking your
  password, even if you save it in a password manager or when using Git Credential Manager.
- Alternatives to using Username/Password have their own flaws:
	- Custom command line tools like Github's `gh` establishes an element of "vendor lock-in" by using that tool. Also,
	  it just sucks to learn an extra thing for something that should be simpler.
	- PATs (Personal Access Tokens) aren't supported by all remote-repository hosts, and are basically just... another
	  type of password that you'll have to manage.

SSH is a more elegant and more secure alternative.

Rather than connecting to your remote repository host (like GitHub) over the HTTPS protocol, you would connect over the
SSH protocol and thus we can take advantage of using SSH's way of handling authentication rather than Username/Password.

SSH uses a "key pair" to do authentication. A key pair is, as it sounds, a pair of keys. Not physical keys, but "
cryptographic" keys. The two keys in a key pair are:

- A Private key
- A Public key

The Private Key is a file that you keep on your computer and keep secret. NEVER give your Private Key to anyone.

The Public Key is a file that is on your computer, that you give out to other people.

When we create key pair, these two keys are associated with one another, which produces the neat result where a piece of
data encrypted with one of the keys can be decrypted with the other key - and since the key is explicitly associated
with you, it can also be used for authentication!

So... bringing this back to Git, if we configure Git to use SSH, we can securely communicate Git changes to & from our
remote repository host, all without needing to send credentials (like you do with Username/Password) to authenticate.

## Configuring Git to use SSH

To configure Git to use SSH, follow the below steps - replacing the email in the commands with your email.

### 1. Generate your SSH key pair

1. Run the following command to begin:
   ```bash
   ssh-keygen -t ed25519 -C "me@example.com"
   ```
2. When asked what file to save it in, just press enter. This will create your key pair in the default file locations:
   `~/.ssh/id_ed25519` for the private key, and `~/.ssh/id_ed25519.pub` for the public key.
3. When asked for a passphrase to file... you can choose to use a passphrase if you want. Personally, I don't bother and
   I don't think you should either (some security-minded folks might not like me for that).

### 2. Add your Private key to your computer's ssh-agent

This is an important step because Git will use the `ssh-agent` system on your computer to handle SSH connections. Whilst
we've now created an SSH key pair, we need to tell the `ssh-agent` system that it's OK to use the Private key in this
new key pair for SSH connections.

1. Start the `ssh-agent` in the background, just in case it's not started:
   ```bash
   eval "$(ssh-agent -s)"
   ```
2. Add your Private key to the `ssh-agent` system:
   ```bash
   ssh-add ~/.ssh/id_ed25519
   ```

### 3. Upload your Public key to your remote repository host

For GitHub, you do this (in 2024) by doing:

1. Get the content of your SSH Public key:
   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```
   It will look something like:
   ```
   ssh-ed25519 Asdflkjhasdflkjhasdflkjhasdflkjhasdflkjhasdflkjh me@example.com
   ```
2. Open https://github.com
3. Go to your profile settings.
4. Go to the "SSH and GPG keys" section.
5. In the SSH keys section, click "New SSH key"
6. Give a title to the key (could be anything, I recommend the name of your computer).
7. For "Key type", choose "Authentication Key".
8. In the "Key" field, paste in the content of your SSH Public key.
9. Click "Add SSH key"

### 4. Configure Git to use SSH

This is as simple as using the SSH version of the URL for your Git repo, rather than the HTTPS version of the URL. For
example:

```bash
# HTTPS:
https://github.com/Harmelodic/website.git

# SSH:
git@github.com:Harmelodic/website.git
```

For GitHub, you can add a remote using the SSH URL by doing:

```bash
git remote add origin git@github.com:Harmelodic/website.git
```

If you already have a remote repository configured, you can switch to using the SSH URL by doing:

```bash
git remote set-url origin git@github.com:Harmelodic/website.git
```

Now you can do `git push` and `git pull` (etc.) and Git will use SSH instead of HTTPS!
