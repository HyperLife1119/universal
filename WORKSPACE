workspace(
    name = "nguniversal",
    managed_directories = {"@npm": ["node_modules"]},
)

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "build_bazel_rules_nodejs",
    sha256 = "e79c08a488cc5ac40981987d862c7320cee8741122a2649e9b08e850b6f20442",
    urls = ["https://github.com/bazelbuild/rules_nodejs/releases/download/3.8.0/rules_nodejs-3.8.0.tar.gz"],
)

# Check the bazel version and download npm dependencies
load("@build_bazel_rules_nodejs//:index.bzl", "check_bazel_version", "check_rules_nodejs_version", "node_repositories", "yarn_install")

# Bazel version must be at least the following version because:
#   - 0.26.0 managed_directories feature added which is required for nodejs rules 0.30.0
#   - 0.27.0 has a fix for managed_directories after `rm -rf node_modules`
check_bazel_version(
    message = """
You no longer need to install Bazel on your machine.
Angular has a dependency on the @bazel/bazelisk package which supplies it.
Try running `yarn bazel` instead.
    (If you did run that, check that you've got a fresh `yarn install`)
""",
    minimum_bazel_version = "4.0.0",
)

check_bazel_version(minimum_bazel_version = "4.0.0")

check_rules_nodejs_version(minimum_version_string = "3.0.0")

# Setup the Node.js toolchain
node_repositories(
    node_version = "14.16.1",
    package_json = ["//:package.json"],
    yarn_version = "1.22.4",
)

yarn_install(
    name = "npm",
    # Ensure that the script is available when running `postinstall` in the Bazel sandbox.
    data = [
        "//:angular-metadata.tsconfig.json",
    ],
    package_json = "//:package.json",
    yarn_lock = "//:yarn.lock",
)
