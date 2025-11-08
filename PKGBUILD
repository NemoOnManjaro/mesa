# Maintainer: Laurent Carlier <lordheavym@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Andreas Radke <andyrtr@archlinux.org>
# Contributor: Dan Johansen <strit@manjaro.org>

# ALARM: Kevin Mihelich <kevin@archlinuxarm.org>
#  - Removed Gallium3D drivers/packages for chipsets that don't exist in our ARM devices (intel, VMware svga).
#  - added broadcom and panfrost vulkan packages
#  - enable lto for aarch64
#  - set -D intel-rt=disabled
#  ALARM: John Audia <therealgraysky@proton.me>
#  - added freedreno driver, PR#1973 and PR#1996

highmem=1

pkgbase=mesa
pkgname=(
  mesa
  opencl-mesa
  vulkan-asahi
  vulkan-dzn
  vulkan-freedreno
  vulkan-gfxstream
  vulkan-nouveau
  vulkan-radeon
  vulkan-swrast
  vulkan-virtio
  vulkan-mesa-device-select
  vulkan-mesa-layers
  vulkan-broadcom
  vulkan-panfrost
)
pkgver=25.2.6
_pkgver=${pkgver/[a-z]/-&}
pkgrel=1
epoch=1
pkgdesc="Open-source OpenGL drivers"
url="https://www.mesa3d.org/"
arch=(x86_64)
license=("MIT AND BSD-3-Clause AND SGI-B-2.0")
makedepends=(
  clang
  directx-headers
  expat
  gcc-libs
  glibc
  libdrm
  libelf
  libglvnd
  libpng
  libva
  libvdpau
  libxcb
  libxext
  libxml2
  llvm
  llvm-libs
  rust
  spirv-llvm-translator
  spirv-tools
  systemd-libs
  vulkan-icd-loader
  wayland
  xcb-util-keysyms
  zlib
  zstd

  # shared between mesa and lib32-mesa
  cbindgen
  clang
  cmake
  elfutils
  glslang
  libclc
  meson
  python-mako
  python-packaging
  python-ply
  python-yaml
  rust-bindgen
  wayland-protocols
  xorgproto

  # mesa-only deps
  libsysprof-capture
  valgrind

  # etnaviv deps
  python-pycparser
)
options=(
  # GCC 14 LTO causes segfault in LLVM under si_llvm_optimize_module
  # https://gitlab.freedesktop.org/mesa/mesa/-/issues/11140
  #
  # In general, upstream considers LTO to be broken until explicit notice.
  !lto
)
source=(
  "https://archive.mesa3d.org/mesa-$_pkgver.tar.xz"{,.sig}

  # Fix DotA 2 hangs
  # https://gitlab.freedesktop.org/mesa/mesa/-/merge_requests/38017
  0001-anv-fix-FS-output-attachment-map-building.patch
)

# Rust crates for NVK, used as Meson subprojects
declare -A _crates=(
  bitflags         2.9.1
  cfg-if           1.0.0
  equivalent       1.0.1
  errno            0.3.12
  hashbrown        0.14.1
  indexmap         2.2.6
  libc             0.2.168
  log              0.4.27
  once_cell        1.8.0
  paste            1.0.14
  pest             2.8.0
  pest_derive      2.8.0
  pest_generator   2.8.0
  pest_meta        2.8.0
  proc-macro2      1.0.86
  quote            1.0.35
  remain           0.2.12
  roxmltree        0.20.0
  rustc-hash       2.1.1
  rustix           1.0.7
  syn              2.0.87
  thiserror        2.0.11
  thiserror-impl   2.0.11
  ucd-trie         0.1.6
  unicode-ident    1.0.12
  zerocopy         0.8.13
  zerocopy-derive  0.8.13
)

# Used to generate the above table
_gencrates() {
  grep crates.io subprojects/*.wrap | \
    sed -r 's|.*crates/([^/]+)/([0-9.]+)/download|\1 \2|' | \
    column -t -S 2 | sed 's/^/  /'
}

for _crate in "${!_crates[@]}"; do
  _ver="${_crates[$_crate]}"
  source+=(
    "$_crate-$_ver.tar.gz::https://crates.io/api/v1/crates/$_crate/$_ver/download"
  )
done

# https://docs.mesa3d.org/relnotes.html
sha256sums=('361c97e8afa5fe20141c5362c5b489040751e12861c186a16c621a2fb182fc42'
            'SKIP'
            '58e81f7d5f38c837f6414f4eb8ba150b21309d02a2060559556e47a3c67cc2fe'
            '67914ab451f3bfd2e69e5e9d2ef3858484e7074d63f204fd166ec391b54de21d'
            'ed646292ffc8188ef8ea4d1e0e0150fb15a5c2e12ad9b8fc191ae7a8a7f3c4b9'
            '7f9f832470494906d1fca5329f8ab5791cc60beb230c74815dff541cbd2b5ca0'
            '5aaeb2981e0606ca11d79718f8bb01164f1d6ed75080182d3abf017e6d244b6d'
            '168fb715dda47215e360912c096649d23d58bf392ac62f73919e831745e40f26'
            '7dfda62a12f55daeae5015f81b0baea145391cb4520f86c248fc615d72640d12'
            '13dc2df351e3202783a1fe0d44375f7295ffb4049267b0f3018346dc122a1d94'
            '7988d73a4303ca289df03316bc490e934accf371af6bc745393cf3c2c5c4f25d'
            '357703d41365b4b27c590e3ed91eabb1b663f07c4c084095e60cbed4362dff0d'
            'baf1de4339761588bc0619e3cbc0120ee582ebb74b53b4efbf79117bd2da40fd'
            '1ad5e011230cad274d0532460c5ab69828ea47ae75681b42a841663efffaf794'
            '5e719e8df665df0d1c8fbfd238015744736151d4445ec0836b8e628aae103b77'
            'd725d9cfd79e87dccc9341a2ef39d1b6f6353d68c4b33c177febbe1a402c97c5'
            '26afc1baea8a989337eeb52b6e72a039780ce45c3edfcc9c5b9d112feeb173c2'
            '3354b9ac3fae1ff6755cb6db53683adb661634f67557942dea4facebec0fee4b'
            '1b8e56985ec62d17e9c1001dc89c88ecd7dc08e47eba5ec7c29c7b5eeecde967'
            '291ec9ab5efd934aaf503a6466c5d5251535d108ee747472c3977cc5acc868ef'
            '6c20b6793b5c2fa6553b250154b78d6d0db37e72700ae35fad9387a46f487c97'
            'de3145af08024dea9fa9914f381a17b8fc6034dfb00f3a84013f7ff43f29ed4c'
            'cea14ef9355e3beab063703aa9dab15afd25f0667c341310c1e5274bb1d0da18'
            'c71e83d6afe7ff64890ec6b71d6a69bb8a610ab78ce364b3352876bb4c801266'
            '198db74531d58c70a361c42201efde7e2591e976d518caf7662a47dc5720e7b6'
            'd452f284b73e6d76dd36758a0c8684b1d5be31f92b89d07fd5822175732206fc'
            '5443807d6dff69373d433ab9ef5378ad8df50ca6298caf15de6e52e24aaf54d5'
            'db7d01726be8ab66ab32f9df467ae8b1148906685bbe75c82d1e65d7f5b3f841'
            '692fcb63b64b1758029e0a96ee63e049ce8c5948587f2f7208df04625e5f6b56'
            '25aa4ce346d03a6dcd68dd8b4010bcb74e54e62c90c573f394c46eae99aba32d')

prepare() {
  cd mesa-$_pkgver

  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    src="${src%.zst}"
    [[ $src = *.patch ]] || continue
    echo "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  # Include package release in version string so Chromium invalidates
  # its GPU cache; otherwise it can cause pages to render incorrectly.
  # https://bugs.launchpad.net/ubuntu/+source/chromium-browser/+bug/2020604
  echo "$_pkgver-arch$epoch.$pkgrel" >VERSION

  if [[ $CARCH == "armv7h" ]]; then
    sed -i "/c_cpp_args += '-mtls-dialect=gnu2'/d" meson.build
  fi
}

build() {
  case "${CARCH}" in
    armv7h)  GALLIUM=",etnaviv,lima,panfrost,tegra,v3d,vc4" ;;
    aarch64) GALLIUM=",etnaviv,lima,panfrost,svga,v3d,vc4" ;;
  esac

  local meson_options=(
    -D android-libbacktrace=disabled
    #-D b_lto=$([[ $CARCH == aarch64 ]] && echo true || echo false)
    -D b_ndebug=true
    -D gallium-drivers=r300,r600,radeonsi,freedreno,nouveau,llvmpipe,softpipe,virgl,zink,d3d12,asahi,freedreno${GALLIUM}
    -D gallium-extra-hud=true
    -D gallium-mediafoundation=disabled
    -D gallium-rusticl=true
    -D gles1=disabled
    -D dri3=disabled
    -D egl=enabled
    -D lmsensors=disabled
    -D osmesa=false
    -D platforms=wayland
    -D shared-llvm=disabled
    -D html-docs=disabled
    -D intel-rt=disabled
    -D libunwind=disabled
    -D microsoft-clc=disabled
    -D sysprof=true
    -D valgrind=enabled
    -D video-codecs=all
    -D xlib-lease=disabled
    -D zstd=disabled
    -D vulkan-drivers=amd,gfxstream,swrast,broadcom,panfrost,virtio,microsoft-experimental,nouveau,asahi,freedreno
    -D vulkan-layers=device-select,overlay,screenshot,vram-report-limit
  )

  # Build only minimal debug info to reduce size
  CFLAGS+=" -g1"
  CXXFLAGS+=" -g1"

  # Inject subproject packages
  export MESON_PACKAGE_CACHE_DIR="$srcdir"

  arch-meson mesa-$_pkgver build "${meson_options[@]}"
  meson compile -C build
}

_pick() {
  local p="$1" f d; shift
  for f; do
    d="$srcdir/$p/${f#$pkgdir/}"
    mkdir -p "$(dirname "$d")"
    mv -v "$f" "$d"
    rmdir -p --ignore-fail-on-non-empty "$(dirname "$f")"
  done
}

package_mesa() {
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    libelf
    libglvnd
    llvm-libs
    spirv-tools
    wayland
    zlib
    zstd
  )
  optdepends=("opengl-man-pages: for the OpenGL API man pages")
  provides=(
    "libva-mesa-driver=$epoch:$pkgver-$pkgrel"
    "mesa-libgl=$epoch:$pkgver-$pkgrel"
    "mesa-vdpau=$epoch:$pkgver-$pkgrel"
    libva-driver
    opengl-driver
    vdpau-driver
  )
  conflicts=(
    'libva-mesa-driver<1:24.2.7-1'
    'mesa-libgl<17.0.1-2'
    'mesa-vdpau<1:24.2.7-1'
  )
  replaces=(
    'libva-mesa-driver<1:24.2.7-1'
    'mesa-libgl<17.0.1-2'
    'mesa-vdpau<1:24.2.7-1'
  )

  meson install -C build --destdir "$pkgdir" --no-rebuild

  (
    local libdir=usr/lib icddir=usr/share/vulkan/icd.d

    cd "$pkgdir"

    _pick opencl $libdir/libRusticlOpenCL*
    _pick opencl etc/OpenCL/vendors/rusticl.icd

    _pick vkasahi $icddir/asahi_icd.*.json
    _pick vkasahi $libdir/libvulkan_asahi.so

    _pick vkd3d12 $icddir/dzn_icd.*.json
    _pick vkd3d12 $libdir/libvulkan_dzn.so
    _pick vkd3d12 $libdir/libspirv_to_dxil.*
    _pick vkd3d12 usr/bin/spirv2dxil

    _pick vkfdreno $icddir/freedreno_icd.*.json
    _pick vkfdreno $libdir/libvulkan_freedreno.so

    _pick vkgfxstr $icddir/gfxstream_vk_icd.*.json
    _pick vkgfxstr $libdir/libvulkan_gfxstream.so

    _pick vknvidia $icddir/nouveau_icd.*.json
    _pick vknvidia $libdir/libvulkan_nouveau.so

    _pick vkradeon $icddir/radeon_icd.*.json
    _pick vkradeon $libdir/libvulkan_radeon.so
    _pick vkradeon usr/share/drirc.d/00-radv-defaults.conf

    _pick vkswrast $icddir/lvp_icd.*.json
    _pick vkswrast $libdir/libvulkan_lvp.so

    _pick vkvirtio $icddir/virtio_icd.*.json
    _pick vkvirtio $libdir/libvulkan_virtio.so

    _pick vkdevice $libdir/libVkLayer_MESA_device_select.so
    _pick vkdevice usr/share/vulkan/implicit_layer.d

    _pick vklayer $libdir/libVkLayer_*.so
    _pick vklayer usr/bin/mesa-*-control.py
    _pick vklayer usr/share/vulkan/explicit_layer.d

    _pick vkbroadcom $icddir/broadcom_icd*.json
    _pick vkbroadcom $libdir/libvulkan_broadcom.so

    _pick vkpanfrost $icddir/panfrost_icd*.json
    _pick vkpanfrost $libdir/libvulkan_panfrost.so

    # indirect rendering
    ln -sr $libdir/libGLX_{mesa,indirect}.so.0
  )
}

package_opencl-mesa() {
  pkgdesc="Open-source OpenCL drivers"
  depends=(
    clang
    expat
    gcc-libs
    glibc
    libdrm
    libelf
    llvm-libs
    spirv-llvm-translator
    spirv-tools
    zlib
    zstd

    libclc # For /usr/share/clc/
  )
  optdepends=("opencl-headers: headers necessary for OpenCL development")
  provides=(opencl-driver)
  replaces=(
    "opencl-clover-mesa<=1:25.0.5-1"
    "opencl-rusticl-mesa<=1:25.0.5-1"
  )
  conflicts=(
    opencl-clover-mesa
    opencl-rusticl-mesa
  )

  mv opencl/* "$pkgdir"
}

package_vulkan-asahi() {
  pkgdesc="Open-source Vulkan driver for Apple GPUs"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkasahi/* "$pkgdir"
}

package_vulkan-dzn() {
  pkgdesc="Open-source Vulkan driver for D3D12"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkd3d12/* "$pkgdir"
}

package_vulkan-freedreno() {
  pkgdesc="Open-source Vulkan driver for Adreno GPUs"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkfdreno/* "$pkgdir"
}

package_vulkan-gfxstream() {
  pkgdesc="Open-source Vulkan driver for Graphics Streaming Kit"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkgfxstr/* "$pkgdir"
}

package_vulkan-nouveau() {
  pkgdesc="Open-source Vulkan driver for Nvidia GPUs"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vknvidia/* "$pkgdir"
}

package_vulkan-radeon() {
  pkgdesc="Open-source Vulkan driver for AMD GPUs"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    libelf
    llvm-libs
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)
  replaces=('amdvlk<=2025.Q2.1-1')

  mv vkradeon/* "$pkgdir"
}

package_vulkan-swrast() {
  pkgdesc="Open-source Vulkan driver for CPUs (Software Rasterizer)"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    llvm-libs
    spirv-tools
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  conflicts=(vulkan-mesa)
  replaces=(vulkan-mesa)
  provides=(vulkan-driver)

  mv vkswrast/* "$pkgdir"
}

package_vulkan-virtio() {
  pkgdesc="Open-source Vulkan driver for Virtio-GPU (Venus)"
  depends=(
    expat
    gcc-libs
    glibc
    libdrm
    systemd-libs
    vulkan-icd-loader
    vulkan-mesa-device-select
    wayland
    xcb-util-keysyms
    zlib
    zstd
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkvirtio/* "$pkgdir"
}

package_vulkan-mesa-device-select() {
  pkgdesc="Mesa's Vulkan Device Select layer"
  depends=(
    glibc
    libdrm
    wayland
  )

  mv vkdevice/* "$pkgdir"
}

package_vulkan-mesa-layers() {
  pkgdesc="Mesa's Vulkan layers"
  depends=(
    gcc-libs
    glibc
    libpng
    python
  )
  conflicts=(vulkan-mesa-layer)
  replaces=(vulkan-mesa-layer)

  mv vklayer/* "$pkgdir"
}

package_vulkan-broadcom() {
  pkgdesc="Broadcom's Vulkan mesa driver"
  depends=(
    wayland
    libxshmfence
    libdrm
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkbroadcom/* "$pkgdir"
}

package_vulkan-panfrost() {
  pkgdesc="Panfrost Vulkan mesa driver"
  depends=(
    wayland
    libxshmfence
    libdrm
  )
  optdepends=("vulkan-mesa-layers: additional vulkan layers")
  provides=(vulkan-driver)

  mv vkpanfrost/* "$pkgdir"
}