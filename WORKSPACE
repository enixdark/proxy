workspace(name = "cilium")

#
# We grep for the following line to generate SOURCE_VERSION file for non-git
# distribution builds. This line must start with the string ENVOY_SHA followed by
# an equals sign and a git SHA in double quotes.
#
# No other line in this file may have ENVOY_SHA followed by an equals sign!
#
ENVOY_SHA = "f936fc60f488cfae07f5e5d20d7381f0f23482fe"

http_archive(
    name = "envoy",
    url = "https://github.com/envoyproxy/envoy/archive/" + ENVOY_SHA + ".zip",
    strip_prefix = "envoy-" + ENVOY_SHA,
)

#
# Bazel does not do transitive dependencies, so we must basically
# include all of Envoy's WORKSPACE file below, with the following
# changes:
# - Skip the 'workspace(name = "envoy")' line as we already defined
#   the workspace above.
# - loads of "//..." need to be renamed as "@envoy//..."
#
load("@envoy//bazel:repositories.bzl", "envoy_dependencies")
load("@envoy//bazel:cc_configure.bzl", "cc_configure")

envoy_dependencies()
cc_configure()

load("@envoy_api//bazel:repositories.bzl", "api_dependencies")
api_dependencies()

load("@io_bazel_rules_go//go:def.bzl", "go_rules_dependencies", "go_register_toolchains")
load("@com_lyft_protoc_gen_validate//bazel:go_proto_library.bzl", "go_proto_repositories")
go_proto_repositories(shared=0)
go_rules_dependencies()
go_register_toolchains()


# Dependencies for Istio filters.
# Cf. https://github.com/istio/proxy.

ISTIO_PROXY_SHA = "490d26feeff134b907f700e08e58a9e6219dce11"

http_archive(
    name = "istio_proxy",
    url = "https://github.com/istio/proxy/archive/" + ISTIO_PROXY_SHA + ".zip",
    strip_prefix = "proxy-" + ISTIO_PROXY_SHA,
)

load("@istio_proxy//:repositories.bzl", "mixerapi_dependencies")
mixerapi_dependencies()

bind(
    name = "boringssl_crypto",
    actual = "//external:ssl",
)
