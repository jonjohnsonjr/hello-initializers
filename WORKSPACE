workspace(name = "io_mattmoor_hello_initializers")

git_repository(
    name = "io_bazel_rules_docker",
    commit = "839a297d4e874216b4fd93f09dd35be5592dc10e",
    remote = "https://github.com/bazelbuild/rules_docker.git",
)

load(
    "@io_bazel_rules_docker//python:image.bzl",
    _py_image_repos = "repositories",
)

_py_image_repos()

git_repository(
    name = "io_bazel_rules_k8s",
    commit = "78a4480e2b092eccf840faf7894c900d1497b9fd",
    remote = "https://github.com/bazelbuild/rules_k8s.git",
)

load("@io_bazel_rules_k8s//k8s:k8s.bzl", "k8s_repositories", "k8s_defaults")

k8s_repositories()

# TODO(mattmoor): Parameterize this.
_CLUSTER = "gke_convoy-adapter_us-central1-a_buildctl"

# No kind, namespaces to the user.
k8s_defaults(
    name = "k8s_object",
    cluster = _CLUSTER,
)

# The CRDs and controllers go into a central namespace.
[k8s_defaults(
    name = "k8s_" + kind,
    cluster = _CLUSTER,
    kind = kind,
    namespace = "hello-initializer",
) for kind in [
    "initializerconfiguration",
    "deployment",
]]


git_repository(
    name = "io_bazel_rules_python",
    commit = "c208292d1286e9a0280555187caf66cd3b4f5bed",
    remote = "https://github.com/bazelbuild/rules_python.git",
)

load(
    "@io_bazel_rules_python//python:pip.bzl",
    "pip_import",
    "pip_repositories",
)

pip_repositories()

pip_import(
    name = "hello_initializers_pip",
    requirements = "//:requirements.txt",
)

load(
    "@hello_initializers_pip//:requirements.bzl",
    _pip_install = "pip_install",
)

_pip_install()
