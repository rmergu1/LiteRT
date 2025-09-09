# Windows-specific pywrap targets
load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(default_visibility = ["//visibility:public"])

config_setting(
    name = "is_windows",
    values = {"cpu": "x64_windows"},
)

# Windows pywrap library
pywrap_library(
    name = "pywrap_litert_windows",
    pywrap_count = 2,
    deps = select({
        ":is_windows": ["//litert/python:litert_pywrap_deps"],
        "//conditions:default": [],
    }),
    visibility = ["//visibility:public"],
)

# Windows binaries
pywrap_binaries(
    name = "pywrap_litert_binaries_windows",
    dep = select({
        ":is_windows": ":pywrap_litert_windows",
        "//conditions:default": None,  # no-op when not Windows
    }),
    visibility = ["//visibility:public"],
)
