# AlmaLinux latest base
FROM almalinux:latest

# 環境変数 (必要なら調整)
ENV ROOT_PASSWORD=okubo 
ENV ADMIN_USER=admin 
ENV ADMIN_PASSWORD=okubo

# 必要パッケージ: telnet-server, xinetd, passwd ユーティリティ
RUN dnf -y update \
    && dnf -y install telnet-server xinetd hostname procps-ng \
    && dnf clean all

# root パスワード設定
RUN echo "root:${ROOT_PASSWORD}" | chpasswd

# 管理ユーザ作成 & パスワード設定
RUN useradd -m -s /bin/bash ${ADMIN_USER} \
    && echo "${ADMIN_USER}:${ADMIN_PASSWORD}" | chpasswd

# xinetd の telnet サービス設定
# AlmaLinux (RHEL系) でも /etc/xinetd.d/telnet の disable を no に変更
RUN sed -i 's/disable\s*=\s*yes/disable = no/' /etc/xinetd.d/telnet || { echo "telnet xinetd file not found"; cat /etc/xinetd.d/*; }

# SELinux が有効な環境の場合のポート許可 (コンテナ内は Permissive/Disabled のことが多いのでコメント)
# RUN yum -y install policycoreutils-python && semanage port -a -t telnet_port_t -p tcp 23 || true

EXPOSE 23

# systemd は使わず xinetd をフォアグラウンドで起動
CMD ["/usr/sbin/xinetd", "-dontfork", "-stayalive", "-f", "/etc/xinetd.conf"]
