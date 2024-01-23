# bazel-flink-example
----------------------

This project demonstrates the usage of Bazel to build a Flink application using Maven.

The bazel java setup was originated from [bazelbuild](https://github.com/bazelbuild/examples/tree/main/java-maven) and the AWS flink setup was taken from [amazon-kinesis-data-analytics-examples](https://github.com/aws-samples/amazon-kinesis-data-analytics-examples)

The project was setup following the instructions from the [AWS page](https://docs.aws.amazon.com/managed-flink/latest/java/get-started-exercise.html)

Build the application by running:

```
bazel build :flink-example
```
The jar file can be found at bazel-bin/flink-example_deploy.jar

Test the application by running:

```
bazel test :tests
```

Create a container image, suitable to push to a remote docker registry:

```
bazel build :image
```

Test that the image works when running inside a container runtime:

```
bazel test :container_test
```