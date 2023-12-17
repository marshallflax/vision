# Docker

## Dockerfile

- Respects `./.dockerignore` 
  - Also `./docker/${DOCKER_FILE_NAME}.Dockerfile.dockerignore`
  - Negate with `!` 
  - Last line to match wins
- Must begin with `FROM` instruction, except comments, parser directives, and global ARGs
  - `#` introduces comments
  - Typical directive is ``# escape=` ``
  - `FROM [--platform=<platform>] <image> [:<tag>|@<digest>] [AS <name>]`
    - Platform defaults to build platform, but `linux/amd64`, `linux/arm64`, `windows/amd64` are common.
    - If neither tag nor digest specified, then docker assumes `latest`; if tag or digest not found, build errors.
    - `[AS <name>]` to allow subsequent `FROM` and `COPY --from=<name>` instructions
  - Clears any state from previous `FROM` (e.g., when a single Dockerfile commits to multiple images)
- `ARG` -- build-time arguments (visible to `docker history`), perhaps with default value
  - Specify with `--build-arg ${VAR}=${VALUE}`
  - Prefer `RUN --mount=type=secret` for secrets!
  - Predefined: `HTTP_PROXY`, `HTTPS_PROXY`, `FTP_PROXY`, `NO_PROXY`, and `ALL_PROXY` (plus all-lower-case)
    - Omitted from `docker history`
  - BuildKit backend provides `{TARGET,BUILD}{PLATFORM,OS,ARCH,VARIANT}` if corresponding `ARG` instruction
- `ENV var=value` -- declares environment variables
  - Access via `$var` or `${var}`
    - `${var:-def}` -- Default value
    - `${var:+setVal}` -- If var is set then return setVal else empty string
  - Supported by `ADD`, `COPY`, `ENV`, `EXPOSE`, `FROM`, `LABEL`, `STOPSIGNAL`, `USER`, `VOLUME`, `WORKDIR`, `ONBUILD` (combined with above)
- `CMD` -- at most one per Dockerfile (otherwise, last wins)
  - Styles
    - Exec style (preferred) -- `CMD ["pathToExecutable", "arg1", "arg2"]` (json syntax, so requires `"`)
      - Does not run within a shell, though `CMD ["sh", "-c", "echo $HOME"]` is of course possible
    - Default parameters to `ENTRYPOINT` -- `CMD ["arg1", "arg2"]`
    - Shell form -- `CMD command arg1 arg2`
      - Even `CMD echo "Hi Mom" | wc -`
  - Overridden by `docker run` arguments
- `ENTRYPOINT` -- at most one per Dockerfile (otherwise, last wins)
  - Should be defined when using the container as an executable
  - `docker run <image>` command-line args are appended to exec-style ENTRYPOINT args, and are ignored for shell-style ENTRYPOINTs
  - Shell form means that SIGTERM kills a `/bin/sh -c` rather the container when a `docker stop` is invoked. (But see <https://community.linuxmint.com/software/view/gosu>)
- `RUN` (shell form (`/bin/sh`) or exec form) -- executes in a new layer on top of the current image and commits results.
  - Fortunately, commits and layers are cheap
- `RUN --mount` -- creates mounts accessible to the build
  - `RUN --mount=type=bind` -- Bind-mount read-only (by default, but written data will be discarded in any case) context directories
  - `RUN --mount=type=cache` -- Temp directories for caching compiler/package-manager data
  - `RUN --mount=type=tmpfs` -- Temp directories for caching compiler/package-manager data
  - `RUN --mount=type=secret` -- Access secure files without inclusion in image
  - `RUN --mount=type=ssh` -- Access ssh keys (via ssh agents), including passphrase support
  - Examples
    - `RUN --mount=type=cache,target=/var/cache/apt,sharing=locked --mount=type=cache,target=/var/lib/apt,sharing=locked apt update && apt-get --no-install-recommends install -y gcc`
    - `RUN --mount=type=secret,id=aws,target=/root/.aws/credentials aws s3 cp s3://... ...`
- `RUN --network=<type>` -- `default`, `none` (only `/dev/lo`), `host` (protected by `network.host` entitlement)
- `RUN --security=<type>` -- Not yet available
- `EXPOSE` -- `EXPOSE 80` or `EXPOSE 80/tcp`, or even `EXPOSE 8000-8080`
  - `docker run -P` or `docker run --publish-all` automatically redirects to higher ports.
  - `docker network` allows communication amongst containers
- `VOLUME` -- Creates a mount point for externally-mounted volumes
  - Will contain anything which is in the created image at the time of the the `VOLUME` instruction
- `ENTRYPOINT`
- `USER` -- Sets the default user for the remainder of the current stage (`RUN`, `ENTRYPOINT`, `CMD`)
  - `USER <user>[:group]` or `USER <uid>[:gid]`
  - `root` group when the specified user doesn't have a primary group.
- `WORKDIR` -- Working dir from `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD`
  - Creates directory if necessary
  - Relative to current WORKDIR unless absolute path specified
- `COPY [--chown=<user>[:<group>]] [--chmod=<perms>] <src> ... <dest>`
  - Fairly typical globbing
  - `<dest>` may be absolute or relative to `WORKDIR`
  - `COPY --link`
- `ADD`
  - Like `COPY` but can handle remote URLs and auto-extract 
- `LABEL <key1>=<val1> <key2>=<val2> <key3>=<val3>` -- Add metadata to image
  - Multiple `LABEL` instructions are fine
  - View with `docker image inspect --format='{{json .Config.Labels}}' myImage`
  - <https://github.com/opencontainers/image-spec/blob/main/annotations.md>
- `ONBUILD ADD`, `ONBUILD RUN` -- instruction to be executed when the current image is a base for another image