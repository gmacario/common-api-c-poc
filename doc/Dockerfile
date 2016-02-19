FROM ubuntu:16.04

apt-get update && apt-get -y dist-upgrade
apt-get -y install autoconf build-essential file git libtool make sudo
apt-get -y install libsystemd-dev pkg-config
id build 2>/dev/null || useradd -m build
echo "build ALL=(ALL) NOPASSWD: ALL" | tee -a /etc/sudoers

# EOF
