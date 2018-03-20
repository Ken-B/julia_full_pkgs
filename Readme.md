
The `.julia` folder is used in the centos6.6 docker container for the julia package directory

run .julia/_zipit.sh to tar.xz all the julia packages with their .git subfolder and copy the REQUIRE and metadata info. Also a _unzipit.sh gets copied

BinDeps creates hard coded full paths for binaries, e.g. in Rmath, so use inside the v0.6/ folder

    find . -type f -exec sed -i 's#/root/#/home/ken/Coding/julia/centos6/#g' {} +

or on mac

    LC_CTYPE=C find . -type f -exec sed -i '' -e "s+/root/+/home/ken/Coding/julia/centos6/+g" {} +


The script to package this was:
outdir="julia_full_pkgs"
mkdir -p $outdir
rm -f "${outdir}/*"
rm -f "${outdir}/.cache"

for dir in .julia/v0.6/*/
do
  base=$(basename "$dir")
  #zip -9 -r "${base}.zip" "$dir" -x *.git*
  #--exclude *.git
  echo "$base"
  tar cJf "${outdir}/${base}.tar.xz" --exclude='.git' --exclude='docs' --exclude='benchmark/.results' --exclude='deps/usr/downloads' "$dir"
done
cp _unzipit.sh "${outdir}"
cp .julia/v0.6/REQUIRE "${outdir}"
cp .julia/v0.6/META_BRANCH "${outdir}"

cp Readme.md ${outdir}
cat _zipit.sh >> "${outdir}/Readme.md"
echo "output of Pkg.status(): " >> "${outdir}/Readme.md"
JULIA_PKGDIR=.julia julia -E 'Pkg.status()' --startup-file=no >> "${outdir}/Readme.md"
output of Pkg.status(): 
17 required packages:
 - BenchmarkTools                0.2.5
 - CSV                           0.1.5
 - CodecBzip2                    0.4.1
 - CodecZlib                     0.4.2
 - Feather                       0.2.5
 - FixedEffectModels             0.4.0
 - Flux                          0.5.1
 - GLM                           0.8.1
 - Gadfly                        0.6.5
 - IJulia                        1.7.0
 - JuliaDB                       0.7.2
 - OhMyREPL                      0.2.11
 - Optim                         0.14.0
 - Plots                         0.15.0
 - PyPlot                        2.5.0
 - RegressionTables              0.0.2
 - StatPlots                     0.7.1
101 additional packages:
 - AbstractTrees                 0.1.0
 - Adapt                         0.2.0
 - AxisAlgorithms                0.2.0
 - BinDeps                       0.8.7
 - BinaryProvider                0.2.5
 - Calculus                      0.2.2
 - CategoricalArrays             0.1.6
 - ColorTypes                    0.6.7
 - Colors                        0.8.2
 - CommonSubexpressions          0.0.1
 - Compat                        0.60.0
 - Compose                       0.5.5
 - Conda                         0.7.1
 - Contour                       0.4.0
 - CoupledFields                 0.0.1
 - Crayons                       0.4.1
 - Dagger                        0.5.2
 - DataArrays                    0.6.2
 - DataFlow                      0.3.1
 - DataFrames                    0.10.1
 - DataStreams                   0.1.3
 - DataStructures                0.7.4
 - DataValues                    0.3.3
 - DiffEqDiffTools               0.4.0
 - DiffResults                   0.0.3
 - DiffRules                     0.0.3
 - Distances                     0.5.0
 - Distributions                 0.15.0
 - DualNumbers                   0.3.0
 - FileIO                        0.7.0
 - FixedPointNumbers             0.4.6
 - FlatBuffers                   0.2.0
 - Formatting                    0.3.0
 - ForwardDiff                   0.7.3
 - GZip                          0.3.0
 - Glob                          1.1.1
 - Hexagons                      0.1.0
 - IndexedTables                 0.6.0
 - Interpolations                0.7.3
 - IterTools                     0.2.1
 - IterableTables                0.7.1
 - IteratorInterfaceExtensions   0.0.2
 - JSON                          0.17.1
 - Juno                          0.4.0
 - KernelDensity                 0.4.1
 - LaTeXStrings                  0.3.0
 - Lazy                          0.12.0
 - LearnBase                     0.1.6
 - LineSearches                  3.2.5
 - Loess                         0.3.0
 - Logging                       0.3.1
 - LossFunctions                 0.2.0
 - MacroTools                    0.4.0
 - MbedTLS                       0.5.8
 - Measures                      0.1.0
 - Media                         0.3.0
 - MemPool                       0.0.7
 - Missings                      0.2.7
 - NLSolversBase                 4.3.0
 - NNlib                         0.3.0
 - NaNMath                       0.3.1
 - NamedTuples                   4.0.0
 - NullableArrays                0.1.2
 - Nullables                     0.0.4
 - OnlineStats                   0.16.0
 - OnlineStatsBase               0.6.1
 - PDMats                        0.8.0
 - Parameters                    0.8.1
 - PenaltyFunctions              0.0.2
 - PlotThemes                    0.2.0
 - PlotUtils                     0.4.4
 - PooledArrays                  0.1.1
 - PositiveFactorizations        0.1.0
 - PyCall                        1.15.0
 - QuadGK                        0.2.0
 - RData                         0.2.0
 - RDatasets                     0.2.0
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
 - StaticArrays                  0.7.0
 - StatsBase                     0.20.1
 - StatsFuns                     0.5.0
 - SweepOperator                 0.1.0
 - TableTraits                   0.2.0
 - TableTraitsUtils              0.1.3
 - TextParse                     0.4.1
 - Tokenize                      0.4.2
 - TranscodingStreams            0.5.1
 - URIParser                     0.3.1
 - WeakRefStrings                0.2.0
 - WoodburyMatrices              0.2.2
 - ZMQ                           0.5.1
 - ZipFile                       0.5.0
nothing
