ARG IMAGE
FROM ${IMAGE:-docker.io/library/centos}:7

ARG ARCH
ARG VERSION
ARG SYSTEMD_ARTIFACTS=./containerd/containerd-for-cc-override.conf
ARG CONTAINER_ENGINE_ARTIFACTS=./scripts
ARG DESTINATION=/opt/confidential-containers-pre-install-artifacts

COPY ${CONTAINERD_ARTIFACTS} ${DESTINATION}/opt/confidential-containers/
COPY ${SYSTEMD_ARTIFACTS} ${DESTINATION}/etc/systemd/system/containerd.service.d/
COPY ${CONTAINER_ENGINE_ARTIFACTS}/* ${DESTINATION}/scripts/

RUN \
curl -fOL --progress-bar https://github.com/confidential-containers/containerd/releases/download/v${VERSION}/containerd-${VERSION}-linux-${ARCH}.tar.gz && \
tar xvzpf containerd-${VERSION}-linux-${ARCH}.tar.gz -C ${DESTINATION}/opt/confidential-containers && \
rm -f containerd-${VERSION}-linux-${ARCH}.tar.gz && \
echo "[kubernetes]" >> /etc/yum.repos.d/kubernetes.repo && \
echo "name=Kubernetes" >> /etc/yum.repos.d/kubernetes.repo && \
echo "baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-$(uname -m)" >> /etc/yum.repos.d/kubernetes.repo && \
echo "gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg" >> /etc/yum.repos.d/kubernetes.repo && \
yum -y install kubectl && \
yum clean all
