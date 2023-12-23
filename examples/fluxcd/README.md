
```shell

## Install flux
curl -s https://fluxcd.io/install.sh | sudo bash


## bootstrap
export GITHUB_TOKEN=$(cat ~/.github/my.pat)

## fine grained needs "contents" and "admin"

flux bootstrap github --token-auth \
  --owner=lancerushing \
  --repository=k8s-getting-started \
  --branch=master \
  --path=examples/fluxcd
```

## Add sops secret

https://fluxcd.io/flux/guides/mozilla-sops/


flux create source git mysql \
--url=https://github.com/lancerushing/k8s-getting-started\
--branch=master


flux create kustomization mysql \
--source=mysql \
--path=./examples/mysql \
--prune=true \
--interval=10m \
--decryption-provider=sops \
--decryption-secret=sops-age


