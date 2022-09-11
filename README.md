# Versions
- OpenCV and opencv_contrib 3.3.1
 copy boostdesc_bgm.i ...  to Extern/opencv_contrib/modules/xfeatures2d/src
- gtsam
git checkout 4.1.1
- Pangolin
git checkout v0.6

# Prepare

- Vocabulary
http://www.dropbox.com/s/lyo0qgbdxn6eg6o/ORBvoc.zip?dl=1
```sh
cd vocabulary
wget -c http://www.dropbox.com/s/lyo0qgbdxn6eg6o/ORBvoc.zip?dl=1
```
# Get External Repos
```sh
cd Extern
wstool init
wstool merge ../kimera.rosinstall
wstool update

# Build
make build

# Run
make up
