# Docker image for genesis
Molecular dynamics software (GENESIS)[http://www.aics.riken.jp/labs/cbrt/] is ready for use

# Usage

```bash
docker run --rm -v $(pwd):/work ymatsunaga/genesis mpirun -np 8 spdyn run.inp
```

