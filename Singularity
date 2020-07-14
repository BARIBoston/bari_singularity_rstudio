# NOTES: 
# * GDAL is not yet available in EPEL 8
# * the GDAL library in CentOS 7 is too old for newer versions of sf
# so: let's try with f32

Bootstrap: docker
From: fedora:32

#Bootstrap: yum
#OSVersion: 7
#MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
#OSVersion: 8
#MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/BaseOS/$basearch/os/
#Include: yum

%labels
    Maintainer ercas

%help
this image will run RStudio with spatial libraries and R packages installed

%post
    export R_LIBRARY=/usr/lib64/R/library # path of the global R library
    export RSTUDIO_RPM=rstudio-server-rhel-1.3.959-x86_64.rpm # RStudio version to use
    export MAKEFLAGS=-j20 # for compiling R packages with multithreading

    echo "updating yum repo and installing basic utilities"
    yum -y update
    yum -y install wget

    echo "installing devtools"
    yum -y groupinstall "Development Tools"

    #echo "installing EPEL repository"
    #yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
    #yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

    echo "installing spatial libraries and development headers"
    # note on installing sf in f32: the configure script incorrectly marks
    # missing sqlite development headers (sqlite-devel, introduced in f32) as
    # missing proj. see the discussion here:
    # https://github.com/r-spatial/sf/issues/1369#issuecomment-623003944
    yum -y install {gdal,geos,libspatialite,proj,sqlite}{,-devel} #proj{-epsg,-nad}

    echo "installing other libraries and development headers"
    yum -y install {openssl,udunits2}{,-devel}

    echo "installing R and R development headers"
    yum -y install R-core{,-devel}

    echo "installing binary RPMs for certain R packages"
    # we install these in hopes that they will bring in any remaining files
    # that we might need, as well as set up the global site-packages folder
    yum -y install {R-Rcpp,R-sp}{,-devel}

    #echo "installing rstudio"
    #wget https://download2.rstudio.org/server/centos8/x86_64/${RSTUDIO_RPM} # CentOS 8
    #wget https://download2.rstudio.org/server/centos6/x86_64/${RSTUDIO_RPM} # CentOS 7 (not 6)
    #yum -y install ${RSTUDIO_RPM}
    #rm -v ${RSTUDIO_RPM}

    echo "installing R packages"
    for package_name in sf; do
        Rscript -e "install.packages('${package_name}', repos='https://cloud.r-project.org', lib='${R_LIBRARY}')"
    done

