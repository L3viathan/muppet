# Muppet

Puppet but smol and different.

- Unlike Ansible:
    - You don't (need) to run this manually
    - It runs on the machine being configured, not some controller.
- Unlike Puppet:
    - You don't need a puppetdb, puppet master, etc. (just a Git remote)
- Unlike all of the above:
    - No arcane language to learn

## Concepts

- `muppet` is a binary that runs every so often (e.g. via a systemd timer)
- You have a Git repository in which you define muppetclasses: essentially
  groups of things to ensure. This could be a deployment of some software
  (including all its dependencies and configurations).
- You assign a host a subset of these muppetclasses to apply.

### Muppetclasses

Muppetclasses are shell scripts. They should:

- Ensure the desired state, if at all possible.
- Don't do things that don't need to be done: Check before doing anything.
- Exit with status 0 if nothing changed, 1 if things did change, and 2 if an error occurred.

To help with these goals, muppet provides a bunch of `ensure-` prefixed binaries:

- `ensure-file`
- `ensure-folder`
- ...

There's also a special binary called `require`, which is used to pull in
dependencies (other muppetclasses). At runtime it does nothing, but muppet
analyses muppetclasses and executes all not-yet-executed dependencies before
executing a muppetclass.
