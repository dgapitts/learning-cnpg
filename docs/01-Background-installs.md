# learning-cnpg


## Background - install kubectl, kind, docker desktop and psql 

Note, you can do this `brew`, but honestly I prefer to do this mostly manually (with the except of `libpq`). 

Installing `kubectl`, as per the [kubernetes.io kubectl-macos install doco](https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/)
```
  503  echo "$(cat kubectl.sha256)  kubectl" | shasum -a 256 --check
  504   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl.sha256"
  505  echo "$(cat kubectl.sha256)  kubectl" | shasum -a 256 --check
  506  chmod +x ./kubectl
  507  sudo mv ./kubectl /usr/local/bin/kubectl
  508  kubectl version --client
  509  kubectl version --client --output=yaml
```

Installing `kind` (and `docker desktop`), as per the [kind.sigs.k8s.io installation doco](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

```
  518  # For Intel Macs
  519  [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-darwin-amd64
  520  # For M1 / ARM Macs
  521  [ $(uname -m) = arm64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-darwin-arm64
  522  chmod +x ./kind
  523  mv ./kind /usr/local/bin/kind
```

You'll need `docker desktop` [mac download here](https://docs.docker.com/desktop/install/mac-install/)
```
  524  which docker
  527  docker ps
```

Now you can now create a k8s cluster via kind
```
  528  kind create cluster --name dbcluster01
  529  kubectl cluster-info --context kind-dbcluster01
  530  kind create cluster --name pg
```

Finally to instal psql (without ), as [discussed in this stackoverflow thread](https://stackoverflow.com/questions/44654216/correct-way-to-install-psql-without-full-postgres-on-macos)

```
brew install libpq
echo 'export PATH="/usr/local/opt/libpq/bin:$PATH"' >> ~/.bash_profile
source ~/.bash_profile
```


## Setup `cnpg` kubectl plugin (via krew) 


As per the [kubectl-plugin doco here](https://cloudnative-pg.io/documentation/1.22/kubectl-plugin/) I used `krew`

```
kubectl krew install cnpg
```
i.e. I first had to install krew

```
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)
```

one gotcha here is that I need to update my `~\.bash_profile` and not  `~\.bashrc`

```
Add the $HOME/.krew/bin directory to your PATH environment variable. To do this, update your .bashrc or .zshrc file and append the following line:

export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"

and restart your shell.
```






