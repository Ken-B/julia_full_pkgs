
The `.julia` folder is used in the centos6.6 docker container for the julia package directory

run .julia/_zipit.sh to tar.xz all the julia packages with their .git subfolder and copy the REQUIRE and metadata info. Also a _unzipit.sh gets copied

BinDeps creates hard coded full paths for binaries, e.g. in Rmath, so use inside the v0.6/ folder

    find . -type f -exec sed -i 's#/root/#/home/ken/Coding/julia/centos6/#g' {} +

or on mac

    LC_CTYPE=C find . -type f -exec sed -i '' -e "s+/root/+/home/ken/Coding/julia/centos6/+g" {} +


The script to package this was:
rm -f julia_full_pkgs/*
rm -f julia_full_pkgs/.cache

for dir in .julia/v0.6/*/
do
  base=$(basename "$dir")
  #zip -9 -r "${base}.zip" "$dir" -x *.git*
  #--exclude *.git
  tar cJf "julia_full_pkgs/${base}.tar.xz" --exclude .git "$dir"
done
cp .julia/_unzipit.sh julia_full_pkgs/
cp .julia/v0.6/REQUIRE julia_full_pkgs/
cp .julia/v0.6/META_BRANCH julia_full_pkgs/

cp Readme.md julia_full_pkgs
cat _zipit.sh >> julia_full_pkgs/Readme.md
echo "output of Pkg.status(): " >> julia_full_pkgs/Readme.md
JULIA_PKGDIR=.julia julia -E 'Pkg.status()' --startup-file=no >> julia_full_pkgs/Readme.md

output of Pkg.status(): 
4 required packages:
 - Feather                       0.3.1
 - FixedEffectModels             0.5.0
 - GLM                           0.10.1
 - OhMyREPL                      0.2.11
28 additional packages:
 - BinDeps                       0.8.6
 - Calculus                      0.2.2
 - CategoricalArrays             0.3.3
 - CodecZlib                     0.4.2
 - Compat                        0.52.0
 - Crayons                       0.4.1
 - DataArrays                    0.7.0
 - DataFrames                    0.11.5
 - DataStreams                   0.3.4
 - DataStructures                0.7.4
 - Distributions                 0.15.0
 - FlatBuffers                   0.3.1
 - Missings                      0.2.6
 - NamedTuples                   4.0.0
 - PDMats                        0.8.0
 - QuadGK                        0.2.0
 - Reexport                      0.1.0
 - Rmath                         0.3.2
 - SHA                           0.5.6
 - SortingAlgorithms             0.2.0
 - SpecialFunctions              0.3.8
 - StatsBase                     0.19.5
 - StatsFuns                     0.5.0
 - StatsModels                   0.2.2
 - Tokenize                      0.4.2
 - TranscodingStreams            0.4.1
 - URIParser                     0.3.0
 - WeakRefStrings                0.4.2
nothing
