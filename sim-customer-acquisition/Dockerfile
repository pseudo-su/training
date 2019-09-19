FROM openjdk:8-jdk-alpine as builder
WORKDIR /root
COPY build.gradle gradlew ./
COPY ./gradle ./gradle
RUN ./gradlew resolveDependencies
COPY . .
RUN ./gradlew build

FROM openjdk:8-jre-alpine as runtime

ENV APPLICATION_USER ktor
ENV APPLICATION_NAME sim-customer-acquisition
RUN adduser -D -g '' $APPLICATION_USER

WORKDIR /app

USER $APPLICATION_USER

COPY --from=builder /root/build/libs/$APPLICATION_NAME.jar ./app.jar


CMD [ \
  "java", \
  "-server", \
  "-XX:+UnlockExperimentalVMOptions", \
  "-XX:+UseCGroupMemoryLimitForHeap", \
  "-XX:InitialRAMFraction=2", \
  "-XX:MinRAMFraction=2", \
  "-XX:MaxRAMFraction=2", \
  "-XX:+UseG1GC", \
  "-XX:MaxGCPauseMillis=100", \
  "-XX:+UseStringDeduplication", \
  "-jar", \
  "app.jar" \
]
