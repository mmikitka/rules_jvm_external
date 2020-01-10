# Override as many Maven settings as possible to create a hermetic environment
genrule(
    name = "mvn",
    srcs = [
        "pom.xml",
        "@maven_genrule_testing//:org_apache_maven_plugins_maven_compiler_plugin",
        "@maven_genrule_testing//:org_apache_maven_plugins_maven_jar_plugin",
        "@maven_genrule_testing//:org_apache_maven_plugins_maven_resources_plugin",
    ],
    outs = ["test.jar"],
    cmd = (
        "TARGET_TMP_DIR=$$(mktemp -d) && " +
        "echo '<settings></settings>' > $${TARGET_TMP_DIR}/settings && " +
        "echo '<toolchains></toolchains>' > $${TARGET_TMP_DIR}/toolchains && " +
        "mvn " +
        "-X " +
        "--global-settings $${TARGET_TMP_DIR}/settings " +
        "--global-toolchains $${TARGET_TMP_DIR}/toolchains " +
        "--settings $${TARGET_TMP_DIR}/settings " +
        "--toolchains $${TARGET_TMP_DIR}/toolchains " +
        "--offline " +
        "--fail-fast " +
        "-f $(execpath :pom.xml) " +
        "-Dmaven.home=$${PWD}/tests/integration " +
        "-Dmaven.multiModuleProjectDirectory=$$PWD " +
        "-Dmaven.repo.local=$(BINDIR)/external/maven_genrule_testing/v1/https/repo.maven.apache.org/maven2 " +
        "compile"
    ),
    toolchains = ["@bazel_tools//tools/jdk:current_java_runtime"],
)
