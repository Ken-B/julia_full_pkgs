
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
  echo "$base"
  tar cjf "julia_full_pkgs/${base}.tar.bz2" --exclude .git "$dir"
done
cp _unzipit.sh julia_full_pkgs/
cp .julia/v0.6/REQUIRE julia_full_pkgs/
cp .julia/v0.6/META_BRANCH julia_full_pkgs/

cp Readme.md julia_full_pkgs
cat _zipit.sh >> julia_full_pkgs/Readme.md
echo "output of Pkg.status(): " >> julia_full_pkgs/Readme.md
JULIA_PKGDIR=.julia julia -E 'Pkg.status()' --startup-file=no >> julia_full_pkgs/Readme.md

output of Pkg.status(): 
19 required packages:
 - BenchmarkTools                0.2.5
 - CSV                           0.2.1
 - CodecBzip2                    0.4.1
 - CodecXz                       0.4.0
 - CodecZlib                     0.4.2
 - CodecZstd                     0.4.0
 - Feather                       0.3.1
 - FixedEffectModels             0.5.0
 - Flux                          0.4.1
 - GLM                           0.10.1
 - GR                            0.25.0
 - IJulia                        1.7.0
 - JuliaDB                       0.6.0
 - Knet                          0.9.0
 - MXNet                         0.3.0
 - OhMyREPL                      0.2.11
 - Optim                         0.13.0
 - Plots                         0.15.0
 - StatPlots                     0.7.0
90 additional packages:
 - Adapt                         0.2.0
 - AutoGrad                      0.0.10
 - AxisAlgorithms                0.2.0
 - BinDeps                       0.8.6
 - Calculus                      0.2.2
 - CategoricalArrays             0.3.4
 - ColorTypes                    0.6.7
 - Colors                        0.8.2
 - CommonSubexpressions          0.0.1
 - Compat                        0.53.0
 - Contour                       0.4.0
 - Crayons                       0.4.1
 - Dagger                        0.5.2
 - DataArrays                    0.7.0
 - DataFlow                      0.3.0
 - DataFrames                    0.11.5
 - DataStreams                   0.3.4
 - DataStructures                0.7.4
 - DataValues                    0.3.1
 - DiffResults                   0.0.3
 - DiffRules                     0.0.3
 - Distances                     0.5.0
 - Distributions                 0.15.0
 - DualNumbers                   0.3.0
 - FixedPointNumbers             0.4.6
 - FlatBuffers                   0.3.2
 - Formatting                    0.3.0
 - ForwardDiff                   0.7.3
 - GZip                          0.3.0
 - Glob                          1.1.1
 - IndexedTables                 0.5.2
 - Interpolations                0.7.3
 - IterTools                     0.2.1
 - IterableTables                0.7.0
 - IteratorInterfaceExtensions   0.0.2
 - JSON                          0.16.4
 - Juno                          0.4.0
 - KernelDensity                 0.4.1
 - Lazy                          0.12.0
 - LearnBase                     0.1.6
 - LineSearches                  3.2.5
 - Loess                         0.3.0
 - Logging                       0.3.1
 - LossFunctions                 0.2.0
 - MacroTools                    0.4.0
 - MbedTLS                       0.5.5
 - Measures                      0.1.0
 - Media                         0.3.0
 - MemPool                       0.0.6
 - Missings                      0.2.6
 - NLSolversBase                 4.2.1
 - NNlib                         0.2.3
 - NaNMath                       0.3.1
 - NamedTuples                   4.0.0
 - Nullables                     0.0.3
 - OnlineStats                   0.15.3
 - OnlineStatsBase               0.5.0
 - PDMats                        0.8.0
 - Parameters                    0.8.1
 - PenaltyFunctions              0.0.2
 - PlotThemes                    0.2.0
 - PlotUtils                     0.4.4
 - PooledArrays                  0.1.1
 - PositiveFactorizations        0.1.0
 - QuadGK                        0.2.0
 - Ratios                        0.2.0
 - RecipesBase                   0.2.3
 - Reexport                      0.1.0
 - Requires                      0.4.3
 - Rmath                         0.3.2
 - SHA                           0.5.6
 - ShowItLikeYouBuildIt          0.2.0
 - Showoff                       0.1.1
 - SortingAlgorithms             0.2.0
 - SpecialFunctions              0.3.8
 - StaticArrays                  0.6.6
 - StatsBase                     0.20.0
 - StatsFuns                     0.5.0
 - StatsModels                   0.2.2
 - SweepOperator                 0.1.0
 - TableTraits                   0.1.0
 - TableTraitsUtils              0.1.3
 - TakingBroadcastSeriously      0.1.1
 - TextParse                     0.4.0
 - Tokenize                      0.4.2
 - TranscodingStreams            0.4.1
 - URIParser                     0.3.0
 - WeakRefStrings                0.4.2
 - WoodburyMatrices              0.2.2
 - ZMQ                           0.5.1
nothing
