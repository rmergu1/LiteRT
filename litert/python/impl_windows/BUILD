load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(default_visibility = ["//visibility:public"])

pywrap_library(
    name = "pywrap_litert_windows",
    pywrap_count = 2,
    deps = ["//litert/python:litert_pywrap_deps"],
    visibility = ["//visibility:public"],
)

pywrap_binaries(
    name = "pywrap_litert_binaries_windows",
    dep = ":pywrap_litert_windows",
    visibility = ["//visibility:public"],
)
