# SSH port forwarding action

GitHub action to forward a remote connection to a local port over SSH.

## Prerequisites

You must have a passwordless SSH key set up on the remote server.

> **Security**
>
> - Supply `ssh-key` as a **secret** in your workflow. The action writes it to `$RUNNER_TEMP` with strict permissions and removes it in a cleanup step.
> - Host key checking is disabled by default (`strict-host-key-checking: no`); this simplifies ephemeral runners but makes you vulnerable to man-in-the-middle attacks. Provide `known-hosts` or set `strict-host-key-checking: yes` in sensitive scenarios.
> - The action cleans up the temporary key on success or failure and kills the SSH process when the job exits.

## Github Action Inputs

| Variable                   | Description                                                  |
| -------------------------- | ------------------------------------------------------------ |
| `ssh-key`                  | SSH private key                                              |
| `ssh-host`                 | SSH host                                                     |
| `ssh-port`                 | SSH port, 1-65535 (default: `22`)                            |
| `ssh-user`                 | SSH user                                                     |
| `local-port`               | Local port (1-65535)                                         |
| `remote-host`              | Remote host                                                  |
| `remote-port`              | Remote port (1-65535)                                        |
| `known-hosts`              | (optional) contents to append to ~/.ssh/known_hosts          |
| `strict-host-key-checking` | (yes/no) SSH StrictHostKeyChecking setting; defaults to `no` |

## Example Usage

```yaml
jobs:
  job_id:
    steps:
      - uses: actions/checkout@v6

      - uses: selfagency/ssh-port-forward-action@v2
        with:
          ssh-key: ${{ secrets.SSH_KEY }}
          ssh-host: your-host.com
          ssh-port: 22
          ssh-user: username
          local-port: 6379
          remote-host: localhost
          remote-port: 6379

      - run: redis-cli -p 6379 ping
```
