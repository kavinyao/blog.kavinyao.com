title: Docker Fails to Start After Power Outage
date: 2020-08-28

---

Recently I [repurposed my NUC](/posts/win10-ubuntu-server/) to a home server. At the moment it's only running a small Django app backed by Postgres. The Postgres instance runs as a Docker container.

The other day there was a random power outage. After booting the server I assumed everything would be up and running given that all of them are managed by systemd. Well, almost – everything was up except Postgres, or more precisely, Docker. This is what the log looks like

    Aug 28 04:41:25 nuc systemd[1]: Started Service for snap application docker.dockerd.
    Aug 28 04:41:25 nuc docker.dockerd[3816]: failed to start daemon: pid file found, ensure docker is not running or delete /var/snap/docker/471/run/docker.pid
    Aug 28 04:41:25 nuc systemd[1]: snap.docker.dockerd.service: Main process exited, code=exited, status=1/FAILURE
    Aug 28 04:41:25 nuc systemd[1]: snap.docker.dockerd.service: Failed with result 'exit-code'.
    Aug 28 04:41:25 nuc systemd[1]: snap.docker.dockerd.service: Scheduled restart job, restart counter is at 3.
    Aug 28 04:41:25 nuc systemd[1]: Stopped Service for snap application docker.dockerd.

Since it was a power outage Docker didn't get a chance to shut down properly and the pid file wasn't cleaned up. The fix is simple: just delete the pid file and start Docker service.

By the way in my instance Docker is installed by Snap but it doesn't matter here.
