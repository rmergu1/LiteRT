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

# Detect Windows
config_setting(
    name = "is_windows",
    values = {"cpu": "x64_windows"},
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

# Cross-platform pywrap target
pywrap_library(
    name = "pywrap_litert",
    common_lib_linkopts = {
        "litert/python/pywrap_litert_common": select({
            ":is_windows": [],
            "//conditions:default": ["-Wl,-rpath,$$ORIGIN/"],
        }),
    },
    common_lib_version_scripts = {
        "litert/python/pywrap_litert_common": select({
            ":is_windows": None,  # disable on Windows
            "//conditions:default": "//tflite/python:pywrap_tflite_common.lds",
        }),
    },
    # 👇 NEW: Disable .def generation on non-Windows
    common_lib_def_files = select({
        ":is_windows": {
            # Windows can specify .def files here if needed
            # "litert/python/pywrap_litert_common": "litert/python/_pywrap_litert_common.def",
        },
        "//conditions:default": {},
    }),
    pywrap_count = 2,  # actual number of pywrap libraries
    visibility = ["//visibility:public"],
    deps = [":litert_pywrap_deps"],
)

# Cross-platform binaries
pywrap_binaries(
    name = "pywrap_litert_binaries",
    dep = ":pywrap_litert",
    visibility = ["//visibility:public"],
)
