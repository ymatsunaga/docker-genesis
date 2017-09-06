# Docker image for genesis
Molecular dynamics software [GENESIS](http://www.aics.riken.jp/labs/cbrt/) is ready for use within docker

# Usage

```bash
docker run --rm -v $(pwd):/work ymatsunaga/genesis mpirun -np 8 spdyn run.inp
```

If permissions become a problem, please add the following options

```bash
docker run --user root -e NB_UID=$(id -u $USER) -e NB_GID=$(id -g $USER) --rm -v $(pwd):/work ymatsunaga/genesis mpirun -np 8 spdyn run.inp
```
