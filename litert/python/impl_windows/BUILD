load("@org_tensorflow//tensorflow:strict.default.bzl", "py_strict_library")
load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(default_visibility = ["//litert:litert_python_users"])

# Dependencies shared by pywrap
py_strict_library(
    name = "litert_pywrap_deps_windows",
    deps = [
        "//litert/python/litert_wrapper/compiled_model_wrapper:_pywrap_litert_compiled_model_wrapper",
        "//litert/python/litert_wrapper/tensor_buffer_wrapper:_pywrap_litert_tensor_buffer_wrapper",
    ],
)

pywrap_library(
    name = "pywrap_litert_windows",
    pywrap_count = 2,
    deps = [":litert_pywrap_deps_windows"],
)

pywrap_binaries(
    name = "pywrap_litert_binaries_windows",
    dep = ":pywrap_litert_windows",
)
