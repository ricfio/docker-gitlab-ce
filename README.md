# GitLab CE (Community Edition) (docker)

Install GitLab CE (Community Edition) self-managed on your host with docker.

## Get started

1. Config the ROOT_PASSWORD and the other environment variables in the .env files:

    ```console
    ROOT_PASSWORD=
    ```

2. Build and Start the GitLab docker container:

    ```bash
    docker-compose up -d
    ```

    ```console
    Creating gitlab ... done
    ...
    ```

3. The initialization process may take a long time, see:

    ```bash
    docker logs -f gitlab
    ```

4. Add gitlab.example.com entry in your /etc/hosts file:

    ```bash
    vi /etc/hosts
    ```

    ```console
    127.0.0.1   gitlab.example.com
    ```

5. Open GitLab admin app in your browser and login with the administrator account:

    - admin-url: [https://gitlab.example.com/admin/](https://gitlab.example.com/admin/)
    - username: root
    - password: (see ROOT_PASSWORD in the .env file)

6. Open GitLab user app in your browser to register and login with a standard user account:

    - users-url: [https://gitlab.example.com/users/](https://gitlab.example.com/users/)

7. Login GitLab docker container (optional):

    ```bash
    docker exec -it gitlab bash
    ```

NOTE:  
This install procedure was tested on 2022-03-26 using Docker-Desktop and WSL2 (Ubuntu) on Windows 10.

## References

- [https://docs.gitlab.com/ee/install/docker.html](https://docs.gitlab.com/ee/install/docker.html)
- [https://hub.docker.com/r/gitlab/gitlab-ce](https://hub.docker.com/r/gitlab/gitlab-ce)
- [https://www.youtube.com/watch?v=BTSf2OoFyqE](https://www.youtube.com/watch?v=BTSf2OoFyqE)

## Appendix

### Initial Root Password

The administrator password will be random generated if you remove the configuration for the initial root password from the docker-compose:

```docker-compose.yml
...
        gitlab_rails['initial_root_password'] = '$ROOT_PASSWORD'
...
```

In this case you can display the initial root password as follow:

```bash
docker exec gitlab cat /etc/gitlab/initial_root_password
```

```console
# WARNING: This value is valid only in the following conditions
#          1. If provided manually (either via `GITLAB_ROOT_PASSWORD` environment variable or via `gitlab_rails['initial_root_password']` setting in `gitlab.rb`, it was provided before database was seeded for the first time (usually, the first reconfigure run).
#          2. Password hasn't been changed manually, either via UI or via command line.
#
#          If the password shown here doesn't work, you must reset the admin password following https://docs.gitlab.com/ee/security/reset_user_password.html#reset-your-root-password.

Password: Q8FxSbKo0v0QzR/oOMG42MyrJu24HOlUXBHeXw6BujU=

# NOTE: This file will be automatically deleted in the first reconfigure run after 24 hours.
```

### To clean the installation

```bash
docker-compose down
sudo find ./etc/gitlab -not \( -name '.gitignore' \) -delete
sudo find ./var/log/gitlab -not \( -name '.gitignore' \) -delete
sudo find ./var/opt/gitlab -not \( -name '.gitignore' \) -delete
```
