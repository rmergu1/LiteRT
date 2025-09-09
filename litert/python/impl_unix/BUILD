load("@org_tensorflow//tensorflow:strict.default.bzl", "py_strict_library")
load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(default_visibility = ["//litert:litert_python_users"])

# Dependencies shared by pywrap
py_strict_library(
    name = "litert_pywrap_deps_unix",
    deps = [
        "//litert/python/litert_wrapper/compiled_model_wrapper:_pywrap_litert_compiled_model_wrapper",
        "//litert/python/litert_wrapper/tensor_buffer_wrapper:_pywrap_litert_tensor_buffer_wrapper",
    ],
)

pywrap_library(
    name = "pywrap_litert_unix",
    common_lib_linkopts = {
        "litert/python/pywrap_litert_common": ["-Wl,-rpath,$$ORIGIN/"],
    },
    common_lib_version_scripts = {
        "litert/python/pywrap_litert_common": "//tflite/python:pywrap_tflite_common.lds",
    },
    pywrap_count = 2,
    deps = [":litert_pywrap_deps_unix"],
)

pywrap_binaries(
    name = "pywrap_litert_binaries_unix",
    dep = ":pywrap_litert_unix",
)
