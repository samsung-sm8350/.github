# LineageOS for Samsung SM8350 Devices

To build LineageOS for Samsung devices powered by the SM8350 SoC, such as the Samsung Galaxy S21 FE (r9q), follow the steps below.

## Prerequisites

- **Operating System:** A 64-bit Linux distribution (e.g., Ubuntu 20.04 LTS) is recommended.
- **Hardware:** At least 16 GB of RAM and 200 GB of free storage.
- **Tools:** Ensure `adb` and `fastboot` are installed and configured on your system.

## Setting Up the Build Environment

1. **Install Required Packages:**
    ```sh
    sudo apt update
    sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32readline-dev lib32z1-dev libelf-dev liblz4-tool libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
    ```

2. **Install Repo Tool:**
    ```sh
    mkdir -p ~/bin
    curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo
    export PATH=~/bin:$PATH
    ```

3. **Configure Git:**
    ```sh
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```

## Downloading the Source Code

1. **Initialize the Repo:**
    ```sh
    mkdir -p ~/android/lineage
    cd ~/android/lineage
    repo init -u https://github.com/LineageOS/android.git -b lineage-22.0
    ```

2. **Create a Roomservice Manifest:**

    Ensure the `.repo/local_manifests/` directory exists:
    ```sh
    mkdir -p .repo/local_manifests
    ```

    #### Method 1: 
    
    Create a file named `roomservice.xml` in the `.repo/local_manifests/` directory with the following content:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <manifest>
        <!-- Samsung SM8350 Common Trees -->
        <project name="samsung-sm8350/android_device_samsung_sm8350-common" path="device/samsung/sm8350-common" remote="github" revision="lineage-22" />
        <project name="samsung-sm8350/android_kernel_samsung_sm8350" path="kernel/samsung/sm8350" remote="github" revision="lineage-22" />
        <!-- Dependencies -->
        <project name="samsung-sm8350/android_device_qcom_common" path="device/qcom/common" remote="github" revision="lineage-22" />
        <project name="samsung-sm8350/android_hardware_samsung" path="hardware/samsung" remote="github" revision="lineage-22.0" />
        <!-- Vendor Tree -->
        <project name="samsung-sm8350/proprietary_vendor_samsung" path="vendor/samsung" remote="github" revision="lineage-22" />
        <!-- Samsung Galaxy S21 FE (r9q) [SM-G990B] -->
        <project name="samsung-sm8350/android_device_samsung_r9q" path="device/samsung/r9q" remote="github" revision="lineage-22" />
    </manifest>
    ```

    #### Method 2:
    Fetch Roomservice Manifest:
    ```sh
    curl -o .repo/local_manifests/roomservice.xml https://raw.githubusercontent.com/samsung-sm8350/.github/refs/heads/main/manifests/roomservice.xml
    ```

4. **Sync the Source Code:**
    ```sh
    repo sync
    ```

## Setup ccache (Optional)
1. **Paste following commands for basic setup:**
    ```sh
    export USE_CCACHE=1
    export CCACHE_EXEC=/usr/bin/ccache
    ccache -M 50G
    ccache -o compression=true
    ```

## Building LineageOS

1. **Set Up the Build Environment:**
    ```sh
    source build/envsetup.sh
    ```

2. **Choose the Target:**
    ```sh
    lunch lineage_r9q-ap3a-eng
    ```

3. **Start the Build:**
    ```sh
    mka bacon
    ```

The compiled ROM will be located in the `out/target/product/r9q/` directory.
