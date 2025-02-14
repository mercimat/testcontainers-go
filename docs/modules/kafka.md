# Kafka (KRaft)

Since testcontainers-go <a href="https://github.com/testcontainers/testcontainers-go/releases/tag/v0.24.0"><span class="tc-version">:material-tag: v0.24.0</span></a>

## Introduction

The Testcontainers module for KRaft: [Apache Kafka Without ZooKeeper](https://developer.confluent.io/learn/kraft).

## Adding this module to your project dependencies

Please run the following command to add the Kafka module to your Go dependencies:

```
go get github.com/testcontainers/testcontainers-go/modules/kafka
```

## Usage example

<!--codeinclude-->
[Creating a Kafka container](../../modules/kafka/examples_test.go) inside_block:runKafkaContainer
<!--/codeinclude-->

## Module reference

The Kafka module exposes one entrypoint function to create the Kafka container, and this function receives two parameters:

```golang
func RunContainer(ctx context.Context, opts ...testcontainers.ContainerCustomizer) (*KafkaContainer, error)
```

- `context.Context`, the Go context.
- `testcontainers.ContainerCustomizer`, a variadic argument for passing options.

### Container Options

When starting the Kafka container, you can pass options in a variadic way to configure it.

#### Image

If you need to set a different Kafka Docker image, you can use `testcontainers.WithImage` with a valid Docker image
for Kafka. E.g. `testcontainers.WithImage("confluentinc/confluent-local:7.5.0")`.

!!! warning
    The minimal required version of Kafka for KRaft mode is `confluentinc/confluent-local:7.4.0`. If you are using an image that
    is different from the official one, please make sure that it's compatible with KRaft mode, as the module won't check
    the version for you.

#### Init script

The Kafka container will be started using a custom shell script:

<!--codeinclude-->
[Init script](../../modules/kafka/kafka.go) inside_block:starterScript
<!--/codeinclude-->

#### Environment variables

The environment variables that are already set by default are:

<!--codeinclude-->
[Environment variables](../../modules/kafka/kafka.go) inside_block:envVars
<!--/codeinclude-->

#### Wait Strategies

If you need to set a different wait strategy for Kafka, you can use `testcontainers.WithWaitStrategy` with a valid wait strategy
for Kafka.

!!!info
    The default deadline for the wait strategy is 60 seconds.

At the same time, it's possible to set a wait strategy and a custom deadline with `testcontainers.WithWaitStrategyAndDeadline`.

#### Docker type modifiers

If you need an advanced configuration for Kafka, you can leverage the following Docker type modifiers:

- `testcontainers.WithConfigModifier`
- `testcontainers.WithHostConfigModifier`
- `testcontainers.WithEndpointSettingsModifier`

Please read the [Create containers: Advanced Settings](../features/creating_container.md#advanced-settings) documentation for more information.

### Container Methods

The Kafka container exposes the following methods:

#### Brokers

The `Brokers(ctx)` method returns the Kafka brokers as a string slice, containing the host and the random port defined by Kafka's public port (`9093/tcp`).

<!--codeinclude-->
[Get Kafka brokers](../../modules/kafka/kafka_test.go) inside_block:getBrokers
<!--/codeinclude-->
