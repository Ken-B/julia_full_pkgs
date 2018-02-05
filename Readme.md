
The `.julia` folder is used in the centos6.6 docker container for the julia package directory

run .julia/_zipit.sh to tar.xz all the julia packages with their .git subfolder and copy the REQUIRE and metadata info. Also a _unzipit.sh gets copied

BinDeps creates hard coded full paths for binaries, e.g. in Rmath, so use inside the v0.6/ folder

    find . -type f -exec sed -i 's#/root/#/home/ken/Coding/julia/centos6/#g' {} +

or on mac

    LC_CTYPE=C find . -type f -exec sed -i '' -e "s+/root/+/home/ken/Coding/julia/centos6/+g" {} +


At one point the _zipit.sh contained

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

and the _unzipit.sh

    mkdir v0.6
    for f in *.tar.xz
    do tar xf $f
    done