FROM base/archlinux:2018.04.01
RUN echo 'Server=https://archive.archlinux.org/repos/2018/04/01/$repo/os/$arch' > /etc/pacman.d/mirrorlist
RUN pacman -Syyuu --noconfirm
RUN pacman -S --needed --noconfirm base-devel sudo
ADD sudoers-pipeline-user /etc/sudoers.d/pipeline
RUN useradd -u 1000 -U pipeline
ADD --chown=pipeline:pipeline libpostal-1.1-alpha /usr/local/src/libpostal-1.1-alpha
WORKDIR /usr/local/src/libpostal-1.1-alpha
ADD makepkg.conf /etc/makepkg.conf
RUN runuser -u pipeline -g pipeline -- makepkg -sri --noconfirm
