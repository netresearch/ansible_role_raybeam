# Raybeam Ansible Role (deprecated)

⚠ **This role is deprecated. Please use [netresearch.docker_containers](https://github.com/netresearch/ansible_role_docker_containers) with the following configuation.** ⚠

```yaml
netresearch_docker_containers:
  - name: "raybeam"
    image: "ghcr.io/netresearch/raybeam:latest"
    command:
      - "raybeam"
      - "serve"
      - "-d"
      - "/raybeam/data/db.bolt"
      - "-s"
      - "ldap://localhost:389"
      - "-b"
      - "DC=example,DC=com"
      - "-u"
      - "readonly"
      - "-p"
      - "readonly"
      - "-g"
      - "CN=Raybeam Admins,OU=Groups,DC=example,DC=com"
    networks:
      - name: "traefik_network"
    mounts:
      - type: bind
        source: "/var/lib/raybeam"
        target: "/raybeam/data"
    ports:
      - "8080:8080"
    labels: "{{ raybeam_container_labels }}"
    restart_policy: unless-stopped
```

> ℹ For more information, please refer to the [documentation of netresearch.docker_containers](https://github.com/netresearch/ansible_role_docker_containers#container-definition).

## What does this role do?

This Ansible role provides an key storage software called [Raybeam](https://github.com/netresearch/raybeam) via Docker.

## Requirements

- Debian 11 (bullseye) / 12 (bookworm)
- Docker on target systems

ℹ️ If you set a port to `"0"` it will not be published.

| Name                         | Default Value                          | Description                |
| ---------------------------- | -------------------------------------- | -------------------------- |
| `raybeam_container_image`    | `"ghcr.io/netresearch/raybeam:latest"` | Docker image name          |
| `raybeam_container_name`     | `"raybeam"`                            | Docker container name      |
| `raybeam_container_networks` | `[]`                                   | Docker container networks  |
| `raybeam_container_labels`   | `{}`                                   | Docker container labels    |
| `raybeam_port`               | `8080`                                 | Raybeam port               |
| `raybeam_ldap_server`        | `"ldap://localhost:389"`               | Raybeam LDAP server        |
| `raybeam_ldap_base_dn`       | `"DC=example,DC=com"`                  | Raybeam LDAP base DN       |
| `raybeam_database_directory` | `"/var/lib/raybeam"`                   | Raybeam database directory |
| `raybeam_database_filename`  | `"db.bolt"`                            | Raybeam database filename  |

## Testing

### Prerequisites

- `ansible`
- `molecule`
- `molecule_vagrant`

For simpler testing we have included molecule tests, which you can run with

```shell
molecule test -- --extra-vars 'docker_registry_username=<DOCKER REGISTRY USERNAME>' --extra-vars 'docker_registry_password=<DOCKER REGISTRY PASSWORD>'
```

For further information we have included the installation guide for `molecule` [here](./molecule/default/INSTALL.rst).
