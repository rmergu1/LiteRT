load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(default_visibility = ["//visibility:public"])

pywrap_library(
    name = "pywrap_litert_unix",
    common_lib_linkopts = {
        "litert/python/pywrap_litert_common": ["-Wl,-rpath,$$ORIGIN/"],
    },
    common_lib_version_scripts = {
        "litert/python/pywrap_litert_common": "//tflite/python:pywrap_tflite_common.lds",
    },
    pywrap_count = 2,
    # ⬇️ Explicitly tell Bazel: no Windows DEF parsing
    win_def_file = None,
    deps = ["//litert/python:litert_pywrap_deps"],
    visibility = ["//visibility:public"],
)

pywrap_binaries(
    name = "pywrap_litert_binaries_unix",
    dep = ":pywrap_litert_unix",
    visibility = ["//visibility:public"],
)
