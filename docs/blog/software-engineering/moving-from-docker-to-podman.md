# Moving from Docker to Podman

## Background

Back on [31st August 2021, Docker announced](https://www.docker.com/blog/updating-product-subscriptions/) a change for
Docker Desktop, where businesses with more than 250 employees or more than $10 million annual revenue, would have to
start a paid subscription with Docker if they wanted to use Docker Desktop.

Since businesses don't like to pay for things, I probably won't be able to use Docker Desktop for work anymore, which is
where most of my development work happens. Also, I don't want different tooling for work and personal stuff, so I'll
change there.

I also write a lot of stuff in Java and like using the Testcontainers library when I need to write integration tests
that require a Test Double to be in place (Databases, Event Busses, etc.), and Testcontainers requires Docker.  
In November 2021,
I [opened a GitHub issue with Testcontainers (Java)](https://github.com/testcontainers/testcontainers-java/issues/4664)
to ask if they could remove Docker as a requirement.  
At time of writing Testcontainers haven't changed, AFAIK.

So to switch over from Docker to something else, my requirements were:

- Support macOS.
- Has to have a decent CLI, ideally like the Docker one.
- Has to support Testcontainers.
- Be free & open-source.
- Be well supported/maintained.
- Be pretty easy to setup & configure.
- I don't care about a GUI.

The options came down to Rancher Desktop, Podman or Colima.

I tried Rancher Desktop first, as it initially seemed like an easy hot-replacement for Docker Desktop, but then I was
met with configuration issues, socket issues, and macOS start-up issues.

I had tried Colima a while back, but the CLI was a bit fiddly, and I know a few other folks have tried to use it and had
configuration & socket issues.

I tried Podman, and it's been seamless... so, Podman it is!

## Migrating

1. Uninstall Docker & Docker Desktop, completely.
3. Install Podman by running:
   ```bash
   brew install podman
   ```
3. Since Podman requires a VM for macOS, we need download and run the Podman VM by running:
   ```bash
   podman machine init
   ```
4. To get the Docker socket aliasing required for Testcontainers, run:
   ```bash
   sudo podman-mac-helper install
   ```
5. Now, start the Podman VM by running:
   ```bash
   podman machine start
   ```
6. Verify Podman socket aliasing is configured:
   ```bash
   ls -la /var/run/docker.sock
   ```
   The output points to a podman.sock file such as:
   ```
   /var/run/docker.sock -> /Users/username/.local/share/containers/podman/machine/podman.sock
   ```
7. Verify Podman is running:
   ```bash
   podman images
   ```

and that's it.

If you restart your machine, then you need to start the Podman VM again by running:

```bash
podman machine start
```

If you need to run a CLI command, use `podman` instead of `docker`. Otherwise the CLI is pretty much the same.

e.g. `podman images` will list your current container images.  
e.g. `podman ps` will list your running containers.

## References

- Podman Docs: https://docs.podman.io/en/latest/
- Podman Mac Helper doc: https://podman-desktop.io/docs/migrating-from-docker/using-podman-mac-helper
- Podman CLI commands: https://docs.podman.io/en/latest/Commands.html

## Linux

I've been using Linux distros a fair bit more recently, specifically Ubuntu. If you need to run containers, podman is
available there too (though since containers are native to Linux, you don't need to mess about with any podman virtual
machine stuff). Just install podman with:

```bash
sudo apt install podman
```

Or if you're using a different distro, whatever package manager is the one you use for your distro.

Happy container-ing!

~ Harmelodic
