# Copyright 2025 Google LLC.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

load("@org_tensorflow//tensorflow:strict.default.bzl", "py_strict_library")
load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

package(
    default_visibility = ["//litert:litert_python_users"],
)

# Common dependencies for pywrap targets
py_strict_library(
    name = "litert_pywrap_deps",
    visibility = ["//visibility:public"],
    deps = [
        "//litert/python/litert_wrapper/compiled_model_wrapper:_pywrap_litert_compiled_model_wrapper",
        "//litert/python/litert_wrapper/tensor_buffer_wrapper:_pywrap_litert_tensor_buffer_wrapper",
    ],
)

# Windows-specific pywrap target
pywrap_library(
    name = "pywrap_litert_windows",
    common_lib_def_files_or_filters = {
        "litert/python/pywrap_litert_common": "//tflite/python:pywrap_tflite_common.json",
    },
    pywrap_count = 2,  # matches actual number of pywrap libraries
    visibility = ["//visibility:public"],
    deps = [
        ":litert_pywrap_deps",
        "//litert/python/litert_wrapper/extra_dep_1",
        "//litert/python/litert_wrapper/extra_dep_2",
        "//litert/python/litert_wrapper/extra_dep_3",
    ],
)

# Unix/macOS-specific pywrap target
pywrap_library(
    name = "pywrap_litert_unix",
    common_lib_linkopts = {
        "litert/python/pywrap_litert_common": ["-Wl,-rpath,$$ORIGIN/"],
    },
    common_lib_version_scripts = {
        "litert/python/pywrap_litert_common": "//tflite/python:pywrap_tflite_common.lds",
    },
    pywrap_count = 2,  # matches actual number of pywrap libraries
    visibility = ["//visibility:public"],
    deps = [
        ":litert_pywrap_deps",
        "//litert/python/litert_wrapper/extra_dep_1",
        "//litert/python/litert_wrapper/extra_dep_2",
        "//litert/python/litert_wrapper/extra_dep_3",
    ],
)

# Windows-specific binaries
pywrap_binaries(
    name = "pywrap_litert_binaries_windows",
    dep = ":pywrap_litert_windows",
    visibility = ["//visibility:public"],
)

# Unix/macOS-specific binaries
pywrap_binaries(
    name = "pywrap_litert_binaries_unix",
    dep = ":pywrap_litert_unix",
    visibility = ["//visibility:public"],
)

# Cross-platform alias to pick correct binaries
alias(
    name = "pywrap_litert_binaries",
    actual = select({
        "@bazel_tools//src/conditions:windows": ":pywrap_litert_binaries_windows",
        "//conditions:default": ":pywrap_litert_binaries_unix",
    }),
    visibility = ["//visibility:public"],
)
