# Openshift Gitea Git Server

Guide on how to create a Gitea git server in Openshift with one user using a template resource.

The difference to the original template is that this Gitea instance has an additional configuration to allow sending webhooks to private IPs: `ALLOWED_HOST_LIST = external,private`.

## Create Project

We will pick `gitea-git-server` as a project name.

```bash
oc new-project gitea-git-server
```

## Create Resources

Hostname should be the route of your Gitea git server.

```bash
oc new-app -f gitea-git-server-template.yaml --param=HOSTNAME="<application-name>-<project>.<default-domain-suffix>"
```

If you are testing with Openshift local and picked the above project you can use this command.

```bash
oc new-app -f gitea-git-server-template.yaml --param=HOSTNAME="gitea-gitea-git-server.apps-crc.testing"
```

## Create User

Create a `gitea` user with a `gitea` password.

```bash
oc exec svc/gitea > /dev/null -- /home/gitea/gitea \
  -w /home/gitea/ \
  -c /home/gitea/conf/app.ini \
  admin user create --username gitea --password gitea --email gitea@gitea.com \
  --must-change-password=false
```

## Resources

- [Gitea for OpenShift](https://github.com/wkulhanek/docker-openshift-gitea)