# ansible-docker-ubuntu

This is a sample template to use ansible with docker.
The ansible server and inventries are ubuntsu(bionic = 18.04).

We prepare ansible server and inventries by docker-compose command. After connecting ansible server, execute ansible command. This command just do download `README.md` of this repository. Really simple😃

## How to use it.

### Preparation

1. Clone this project in your PC.
    ```bash
    git clone https://github.com/koheitakumi/ansible-docker-ubuntu.git
    ```
1. Install Docker.
    - If you use Mac or Windows, you can use [Docker Desktop](https://www.docker.com/products/docker-desktop).
1. Generate a SSH key pair.
    - The secret key is for the ansible server to access inventries.
    - The public key is for the inventries to accept the ansible server.

    1. Generate key pair for this sample.
        ```bash
        ssh-keygen -t ed25519 -N "" -f ~/.ssh/test_id_rsa
        ```
        If you're machine is windows, please check [this article](https://docs.joyent.com/public-cloud/getting-started/ssh-keys/generating-an-ssh-key-manually/manually-generating-your-ssh-key-in-windows). I recommend you to use Git Bash because I suppose you have already install it to use git.
    1. Copy the content to the file.
        - Secret key: Copy the content of test_id_rsa and paste it to docker/ansible/id_rsa
        - Public key: Copy the content of test_id_rsa.pub and paste it to docker/target/id_rsa.pub

### Build and run

1. Build containers with docker-compose.
    ```bash
    docker-compose up -d
    ```
1. Check the containers which just are built.
    ```bash
    docker ps
    ```
    ```bash
    # result of `docker ps`

    CONTAINER ID        IMAGE                            COMMAND                  CREATED             STATUS              PORTS               NAMES
    d3a99ef8ab16        ansible-docker-ubuntu_target01   "/usr/sbin/sshd -D"      20 minutes ago      Up 20 minutes       22/tcp              target01
    aee0ebbba82e        ansible-docker-ubuntu_ansible    "/bin/sh -c /bin/bash"   20 minutes ago      Up 20 minutes                           ansible
    dadfcdc95fa9        ansible-docker-ubuntu_target02   "/usr/sbin/sshd -D"      20 minutes ago      Up 20 minutes       22/tcp              target02
    ```
1. Connect the ansible server.
    ```bash
    docker exec -it ansible sh
    ```
1. Confirm that you can connect target01 and 02 server from ansible server.
    ```bash
    # At first, please type `yes`, when you are confirmed to connect with ssh.
    ssh target01

    # After connect target01
    exit
    ```
    ```bash
    ssh target02

    exit
    ```
1. Execute ansible command to the target01 and 02.
    ```bash
    # On the ansible server.
    ansible-playbook -i inventry.ini playbook.yml
    ```
    ```bash
    # You can find the following message on success.

    TASK [Test] ********************************************************************************************************************************************************
    [WARNING]: Consider using the get_url or uri module rather than running 'wget'.  If you need to use command because get_url or uri is insufficient you can add
    'warn: false' to this command task or set 'command_warnings=False' in ansible.cfg to get rid of this message.
    changed: [target01]
    changed: [target02]

    PLAY RECAP *********************************************************************************************************************************************************
    target01                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    target02                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    ```
1. Check whether the README.md file exists on each target01 and 02.
    ```bash
    ssh target01

    ls
    # README.md

    exit
    ```
    ```bash
    ssh target02

    ls
    # README.md

    exit
    ```

### Clean this project up

1. Disconnect ansible server.
    ```bash
    exit
    ```
1. On the host, stop containers.
    ```bash
    docker-compose down
    ```
    ```bash
    # result

    Stopping target01 ... done
    Stopping ansible  ... done
    Stopping target02 ... done
    Removing target01 ... done
    Removing ansible  ... done
    Removing target02 ... done
    Removing network ansible-docker-ubuntu_default
    ```
1. Check the status of containers if necessary.
    ```bash
    docker ps -a
    ```
1. Remove all images
    ```bash
    docker rmi ansible-docker-ubuntu_ansible ansible-docker-ubuntu_target01 ansible-docker-ubuntu_target02
    ```
1. Check the images if necessary.
    ```bash
    docker images
    ```

Enjoy ansible with docker!
