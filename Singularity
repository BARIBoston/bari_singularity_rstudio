# NOTE: CentOS 8 is the version used on Discovery, but there are some package
# conflicts when trying to install the spatial libraries. i have included the
# code to build with CentOS 8, for when those conflicts get resolved. -ercas

Bootstrap: yum
OSVersion: 7
MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/os/$basearch/
#OSVersion: 8
#MirrorURL: http://mirror.centos.org/centos-%{OSVERSION}/%{OSVERSION}/BaseOS/$basearch/os/
Include: yum

%labels
  Maintainer ercas

%help
this image will run RStudio with spatial libraries installed

%post
  export RSTUDIO_RPM=rstudio-server-rhel-1.3.959-x86_64.rpm

  echo "installing basic utilities"
  yum -y install wget

  echo "installing devtools"
  yum -y groupinstall "Development Tools"

  #echo "installing EPEL repository"
  # CentOS 8 (package conflicts)
  #yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  echo "installing EPEL repository"
  yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

  echo "installing spatial libraries"
  yum -y install gdal gdal-devel proj proj-devel geos geos-devel

  echo "installing rstudio"
  # CentOS 8
  #wget https://download2.rstudio.org/server/centos8/x86_64/${RSTUDIO_RPM}
  # CentOS 7
  wget https://download2.rstudio.org/server/centos6/x86_64/${RSTUDIO_RPM}
  yum -y install ${RSTUDIO_RPM}
  rm -v ${RSTUDIO_RPM}

  echo "installing R"
  yum -y install R

  echo "installing R packages"
