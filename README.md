# doker for genesis
molecular dynamics software GENESIS docker image

# usage
docker run --rm -v $(pwd):/work ymatsunaga/genesis mpirun -np 8 spdyn run.inp

