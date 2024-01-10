Exposed neo4j (defaults to port 7687) - can change port by setting $NEOPORT

Update your ~/.nxc/nxc.conf with the default credations
```console
[BloodHound]
bh_enabled = True
bh_uri = 127.0.0.1
bh_port = 7687
bh_user = neo4j
bh_pass = bloodhoundcommunityedition
```
If using through proxychains remember to route the port back locally.
`ssh root@TARGETIP -R 7687:localhost:7687`

Bloodhound-ce doesn't natively have owned user support, so custom cyphers will have to be added/used.
Bloodhound.py is broken currently due to Bloodhound reverting field types. You will need to manually change the fields to their string counterparts to import.
https://github.com/dirkjanm/BloodHound.py/issues/157
 
 Single User BloodHound CE
==========================

This runs [BloodHound CE](https://github.com/SpecterOps/BloodHound) as if it
were a single, self-contained app in a single-user scenario.

It's based on SpecterOp's
[Dockerfile](https://github.com/SpecterOps/BloodHound/blob/294dab1f72fb3fcbaf7d010fd7ee9301f6ba78fe/dockerfiles/bloodhound.Dockerfile),
but uses podman, sets the default credentials to **admin/admin** (no password
change needed) and exposes port 8181 on localhost only.

No dependencies except for podman (and `bash`, `grep` and `date`)!

Simply run `./bloodhound-su`. Link or copy it to `~/.local/bin` or
`/usr/bin` if you want.

It supports workspaces to keep different databases in parallel. They're
located in `$XDG_DATA_HOME/SingleUserBloodHound`
(or `~/.local/share/SingleUserBloodHound` by
default). To set the name of the workspace, use environment variables:

```console
$ WORKSPACE=client1 bloodhound-su
```

The location of the workspace's data directory can be set directly like so:

```console
$ DATA_DIR=BH_DATA bloodhound-su
```

Then the data will be stored in `BH_DATA` in the current working directory.
The port to listen on can similarly be changed by setting `$PORT`.

In case you want to start over completely, delete the containers and volumes:
```console
$ podman container rm --filter name='SingleUserBloodHound*'
$ rm -rf ~/.local/share/SingleUserBloodHound/
```
