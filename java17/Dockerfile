FROM mcr.microsoft.com/vscode/devcontainers/java:17-bookworm AS builder
ARG INSTALL_MAVEN="true"
ARG MAVEN_VERSION="3.9.3"

ARG INSTALL_GRADLE="false"
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk update && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi && curl https://start.spring.io/starter.tgz -d dependencies=web,actuator,lombok -d bootVersion=2.7.14 -d name=simple -d type=maven-project | tar -xzvf - && mvn clean package

FROM mcr.microsoft.com/vscode/devcontainers/java:17-bookworm
ARG INSTALL_MAVEN="true"
ARG MAVEN_VERSION="3.9.3"
ARG INSTALL_GRADLE="false"
COPY kubectl-helm-debian.sh /tmp/library-scripts/
RUN if [ "${INSTALL_MAVEN}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install maven \"${MAVEN_VERSION}\""; fi \
    && if [ "${INSTALL_GRADLE}" = "true" ]; then su vscode -c "umask 0002 && . /usr/local/sdkman/bin/sdkman-init.sh && sdk install gradle \"${GRADLE_VERSION}\""; fi && curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash && curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && bash /tmp/library-scripts/kubectl-helm-debian.sh
COPY --from=builder /root/.m2 /root/.m2
