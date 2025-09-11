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

# ---------------------------------------------------------------------------
# Load macros from TensorFlow's Bazel extensions
# ---------------------------------------------------------------------------
load("@org_tensorflow//tensorflow:strict.default.bzl", "py_strict_library")
load("@org_tensorflow//tensorflow:tensorflow.default.bzl", "pywrap_binaries", "pywrap_library")

# By default, only expose to litert's Python users
package(
    default_visibility = ["//litert:litert_python_users"],
)

# ---------------------------------------------------------------------------
# Platform detection
# ---------------------------------------------------------------------------
config_setting(
    name = "is_windows",
    values = {"cpu": "x64_windows"},
)

# ---------------------------------------------------------------------------
# Workaround: Stub targets to prevent WinDefFileParse on non-Windows
# ---------------------------------------------------------------------------

# Generates a harmless empty .def file.
genrule(
    name = "empty_def",
    outs = ["empty.def"],
    cmd = "mkdir -p $(dirname $@) && echo -n > $@",
    visibility = ["//visibility:public"],
)

# Filegroup that matches the expected _pywrap_litert_common_def target.
# On non-Windows, it provides the empty file so Bazel doesn't try to run
# no_op.bat (which is Windows-only). On Windows, it does nothing so the
# original pywrap rules still run normally.
filegroup(
    name = "_pywrap_litert_common_def",
    srcs = select({
        ":is_windows": [],                 # Windows builds → leave empty
        "//conditions:default": [":empty_def"],  # Non-Windows builds → stub file
    }),
    visibility = ["//visibility:private"],
)

# ---------------------------------------------------------------------------
# Dependencies used by pywrap_library
# ---------------------------------------------------------------------------
py_strict_library(
    name = "litert_pywrap_deps",
    visibility = ["//visibility:public"],
    deps = [
        "//litert/python/litert_wrapper/compiled_model_wrapper:_pywrap_litert_compiled_model_wrapper",
        "//litert/python/litert_wrapper/tensor_buffer_wrapper:_pywrap_litert_tensor_buffer_wrapper",
    ],
)

# ---------------------------------------------------------------------------
# Cross-platform pywrap target
# ---------------------------------------------------------------------------
pywrap_library(
    name = "pywrap_litert",
    # Linker options for the common library
    common_lib_linkopts = {
        "litert/python/pywrap_litert_common": select({
            ":is_windows": [],  # No rpath tweaks on Windows
            "//conditions:default": ["-Wl,-rpath,$$ORIGIN/"],
        }),
    },
    # Version scripts for symbol visibility
    common_lib_version_scripts = {
        "litert/python/pywrap_litert_common": select({
            ":is_windows": None,  # Disable on Windows
            "//conditions:default": "//tflite/python:pywrap_tflite_common.lds",
        }),
    },
    pywrap_count = 2,  # number of pywrap libraries in this package
    visibility = ["//visibility:public"],
    deps = [":litert_pywrap_deps"],
)

# ---------------------------------------------------------------------------
# Cross-platform pywrap binaries
# ---------------------------------------------------------------------------
pywrap_binaries(
    name = "pywrap_litert_binaries",
    dep = ":pywrap_litert",
    visibility = ["//visibility:public"],
)
