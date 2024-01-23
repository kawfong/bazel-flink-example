load("@aspect_bazel_lib//lib:tar.bzl", "tar")
load("@container_structure_test//:defs.bzl", "container_structure_test")
load("@rules_java//java:defs.bzl", "java_binary", "java_library", "java_test")
load("@rules_oci//oci:defs.bzl", "oci_image")

package(default_visibility = ["//visibility:public"])

java_library(
    name = "flink-example-lib",
    srcs = glob(["src/main/java/com/example/myproject/*.java"]),
    deps = [
        "@maven//:com_amazonaws_aws_kinesisanalytics_runtime",
        "@maven//:com_google_guava_guava",
        "@maven//:org_apache_flink_flink_connector_aws_kinesis_streams",
        "@maven//:org_apache_flink_flink_connector_kinesis",
        "@maven//:org_apache_flink_flink_core",
        "@maven//:org_apache_flink_flink_streaming_java",
    ],
)

java_binary(
    name = "flink-example",
    main_class = "com.example.myproject.BasicStreamingJob",
    runtime_deps = [":flink-example-lib"],
)

java_test(
    name = "tests",
    srcs = glob(["src/test/java/com/example/myproject/*.java"]),
    test_class = "com.example.myproject.TestApp",
    deps = [
        ":flink-example-lib",
        "@maven//:com_google_guava_guava",
        "@maven//:junit_junit",
    ],
)

tar(
    name = "layer",
    srcs = ["flink-example_deploy.jar"],
)

oci_image(
    name = "image",
    base = "@distroless_java",
    entrypoint = [
        "java",
        "-jar",
        "/flink-example-deploy.jar",
    ],
    tars = [":layer"],
)

container_structure_test(
    name = "container_test",
    configs = ["container-structure-test.yaml"],
    image = ":image",
    tags = ["requires-docker"],
)
