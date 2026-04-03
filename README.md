# authorized-keys

This repository manages SSH public keys used to authenticate developers on all EE-SJSU Ansible-managed machines. Add your public key here to gain access — an admin will propagate it to all target machines.

## How It Works

Each `.pub` file in the `ansible/` directory maps to an authorized developer. After any change is committed, an admin must manually run a playbook to push updates to all managed machines.

## Adding Your Key

1. Generate a key pair (skip if you already have one):
   ```bash
   ssh-keygen -t ed25519 -f ~/.ssh/ansible-<initials>
   ```

2. Copy `~/.ssh/ansible-<initials>.pub` into the `ansible/` directory in this repo.

3. Commit and push.

## Removing a Key

1. Delete `ansible/<initials>.pub`
2. Commit and push.

## Propagating Changes (Admins Only)

> ⚠️ Changes do **not** auto-propagate. An admin with existing access must run the playbook after every update.

```bash
ansible-playbook maintenance/update_authorized_keys.yaml
```

This playbook updates authorized keys on all managed machines (as root, via the `ansible` remote user).

**Run this after:**
- Adding a new key
- Removing a revoked key
- Updating an existing key

## Repository Structure

```
authorized-keys/
└── ansible/
    ├── key1.pub        # Admin 1 for ansible
    └── key2.pub        # Admin 2 for ansible
```

## Security Rules

- **Never commit private keys** — `.pub` files only
- **Verify identity** before approving a new key
- **Revoke promptly** — remove keys as soon as access should be removed
- **Use ed25519** — preferred over RSA

## Related Repositories

- [ansible-machine-configurations](https://github.com/EE-SJSU/ansible-machine-configurations)
- [EE-Ansible-Collection](https://github.com/EE-SJSU/EE-Ansible-Collection)
