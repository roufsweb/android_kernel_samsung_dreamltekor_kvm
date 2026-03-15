# Samsung Galaxy S8 Korea variant (dreamltekor) KVM enabled test kernel

This kernel is KVM enabled kernel for testing and development.
It is explicitly designed for the **SM-G950N** (Galaxy S8 Korean variant).

## Key Features & Fixes
* **Virtualization (KVM) Enabled**: Hardware-assisted virtualization for running guests.
* **Built-in dtbtool**: Included dtbtool for packing the `boot.img` effortlessly.
* **Integrated Toolchain**: The `aarch64-linux-android-4.9` toolchain is bundled in `scripts/toolchain`, nullifying the need to download it externally.

---

## Build Environment Requirements

As this kernel is based on Linux 4.4 and requires the older GCC 4.9 toolchain, it is **highly recommended** to build this on an older Linux distribution for maximum compatibility.

### Recommended OS:
* **Ubuntu 18.04 LTS (Bionic Beaver)** - *Best Compatibility*
* **Ubuntu 20.04 LTS (Focal Fossa)** - *Works if legacy packages are installed*

### Required Dependencies:
To ensure a smooth build without missing library errors, run the following setup commands to install all necessary dependencies.

```bash
sudo apt-get update
sudo apt-get install -y build-essential libncurses5-dev bzip2 bc \
    bison flex libssl-dev lzop git python3 curl software-properties-common

# If you are on Ubuntu 20.04+, you may also need to explicitly install older ncurses combat libs:
sudo apt-get install -y libncurses5 libncursesw5
```

---

## How to Build

1. **Clone the repository and submodules:**
   Use the following command to download the source, including the Android Image Kitchen (`AIK-Linux`) submodule.
   ```bash
   git clone --recursive https://github.com/roufsweb/android_kernel_samsung_dreamltekor_kvm.git
   cd android_kernel_samsung_dreamltekor_kvm
   ```
   *(If you've already cloned without submodules, run: `git submodule update --init --recursive`)*

2. **Prepare the original `boot.img`:**
   You must provide an original Galaxy S8 `boot.img` (from stock firmware or TWRP backup) and place it in the `AIK-Linux` folder. Unpack it using:
   ```bash
   cd AIK-Linux
   ./unpackimg.sh boot.img
   cd ..
   ```
   This will extract the ramdisk and image structures into `AIK-Linux/split_img/`.

3. **Start the build process:**
   Execute the customized build script. It will automatically clean the previous build, apply the correct `defconfig`, and compile the kernel utilizing all available CPU cores.
   ```bash
   bash build_kernel.sh
   ```

4. **Retrieve the Output:**
   During the packaging stage at the end of `build_kernel.sh`, it will ask you to name the final `.img` file. Provide a name (e.g., `boot-kvm.img`).
   Your new, flashable KVM boot image will be located in the `AIK-Linux/` directory.

---

## Credits
* [Introduce Exynos dtbtool to the Linux build system](https://github.com/jcadduono/android_kernel_samsung_universal8895/commit/945a8df88cf1d783407a36b660e8c767634a1968)
* AIK-Linux by [osm0sis](https://xdaforums.com/t/tool-android-image-kitchen-unpack-repack-kernel-ramdisk-win-android-linux-mac.2073775/)
