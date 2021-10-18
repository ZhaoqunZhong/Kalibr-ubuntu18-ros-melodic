# Installation in Ubuntu18

## 1. Install ros melodic

Afterwards, add 
```
source /opt/ros/melodic/setup.bash 
```
in ~.bashrc file.

## 2. Install dependencies

sudo apt-get install python-setuptools python-rosinstall ipython libeigen3-dev libboost-all-dev doxygen libopencv-dev

sudo apt-get install ros-melodic-vision-opencv ros-melodic-image-transport-plugins ros-melodic-cmake-modules 

sudo apt-get install software-properties-common libpoco-dev python-matplotlib python-scipy python-git python-pip ipython 

sudo apt-get install libtbb-dev libblas-dev liblapack-dev python-catkin-tools libv4l-dev

pip install -i https://pypi.tuna.tsinghua.edu.cn/simple python-igraph==0.7.1-4

## 3. Create a catkin workspace
```
mkdir -p ~/kalibr_workspace/src
cd ~/kalibr_workspace
source /opt/ros/melodic/setup.bash
catkin init
catkin config --extend /opt/ros/melodic
catkin config --merge-devel # Necessary for catkin_tools >= 0.4.
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release
```

Modfiy ~/kalibr_workspace/src/kalibr/suitesparse/CMakeLists.txt to use pre-downloaded SuiteSparse-${VERSION}.tar.gz, check detail in related file.
```
ExternalProject_Add(suitesparse_src
  CMAKE_ARGS -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER} -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
  DOWNLOAD_COMMAND rm -f SuiteSparse-${VERSION}.tar.gz #&& wget http://faculty.cse.tamu.edu/davis/SuiteSparse/SuiteSparse-${VERSION}.tar.gz
  PATCH_COMMAND tar -xzf ~/kalibr_workspace/src/kalibr/SuiteSparse-${VERSION}.tar.gz && rm -rf ../suitesparse_src-build/SuiteSparse && sed -i.bu "s/\\/usr\\/local\\/lib/..\\/lib/g" SuiteSparse-${VERSION}/SuiteSparse_config/SuiteSparse_config.mk && sed -i.bu "s/\\/usr\\/local\\/include/..\\/include/g" SuiteSparse-${VERSION}/SuiteSparse_config/SuiteSparse_config.mk && mv SuiteSparse-${VERSION} ../suitesparse_src-build/
  CONFIGURE_COMMAND ""
  BUILD_COMMAND cd SuiteSparse-${VERSION} && make library -j8 -l8
  INSTALL_COMMAND cd SuiteSparse-${VERSION} && mkdir -p lib && mkdir -p include && make install && cd lib && cp libamd.2.3.1.a libcamd.2.3.1.a libcholmod.2.1.2.a libcxsparse.3.1.2.a libldl.2.1.0.a libspqr.1.3.1.a libumfpack.5.6.2.a libamd.a	libcamd.a libcholmod.a	libcxsparse.a libldl.a libspqr.a libumfpack.a libbtf.1.2.0.a	libccolamd.2.8.0.a libcolamd.2.8.0.a libklu.1.2.1.a librbio.2.1.1.a libsuitesparseconfig.4.2.1.a libbtf.a	libccolamd.a libcolamd.a		libklu.a librbio.a libsuitesparseconfig.a  ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_LIB_DESTINATION}/ && cd .. && mkdir -p ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}/suitesparse && cd include && cp amd.h cholmod_matrixops.h SuiteSparseQR_definitions.h umfpack_load_symbolic.h umfpack_save_symbolic.h btf.h cholmod_modify.h SuiteSparseQR.hpp umfpack_numeric.h umfpack_scale.h camd.h cholmod_partition.h umfpack_col_to_triplet.h umfpack_qsymbolic.h umfpack_solve.h ccolamd.h cholmod_supernodal.h umfpack_defaults.h umfpack_report_control.h umfpack_symbolic.h cholmod_blas.h cholmod_template.h umfpack_free_numeric.h umfpack_report_info.h umfpack_tictoc.h cholmod_camd.h colamd.h umfpack_free_symbolic.h umfpack_report_matrix.h umfpack_timer.h cholmod_check.h cs.h umfpack_get_determinant.h umfpack_report_numeric.h umfpack_transpose.h cholmod_cholesky.h klu.h umfpack_get_lunz.h umfpack_report_perm.h umfpack_triplet_to_col.h cholmod_complexity.h ldl.h umfpack_get_numeric.h umfpack_report_status.h umfpack_wsolve.h cholmod_config.h RBio.h umfpack_get_symbolic.h umfpack_report_symbolic.h cholmod_core.h spqr.hpp umfpack_global.h umfpack_report_triplet.h cholmod.h SuiteSparse_config.h umfpack.h umfpack_report_vector.h cholmod_io64.h SuiteSparseQR_C.h umfpack_load_numeric.h umfpack_save_numeric.h ${CATKIN_DEVEL_PREFIX}/${CATKIN_GLOBAL_INCLUDE_DESTINATION}/suitesparse 
)
```

## 4. Build
```
cd ~/kalibr_workspace
catkin build -DCMAKE_BUILD_TYPE=Release -j4
```

Afterwards, add
```
source ~/kalibr_workspace/devel/setup.bash
```
in ~.bashrc file.

## 5. Bug fixes

(1) Change line 8 of ~/kalibr_workspace/src/kalibr/aslam_offline_calibration/kalibr/python/kalibr_bagcreater, check detail in related file.
```
from PIL import ImageFile # import ImageFile
```

