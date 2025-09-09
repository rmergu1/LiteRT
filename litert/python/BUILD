# Copyright 2025 Google LLC.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
# either express or implied.
# See the License for the specific language governing permissions
# and limitations under the License.

load("@org_tensorflow//tensorflow:strict.default.bzl", "py_strict_library")

package(
    default_visibility = ["//litert:litert_python_users"],
)

# ----------------------------------------------------------------------
# Common dependencies
# ----------------------------------------------------------------------
py_strict_library(
    name = "litert_pywrap_deps",
    visibility = ["//visibility:public"],
    deps = [
        "//litert/python/litert_wrapper/compiled_model_wrapper:_pywrap_litert_compiled_model_wrapper",
        "//litert/python/litert_wrapper/tensor_buffer_wrapper:_pywrap_litert_tensor_buffer_wrapper",
    ],
)

# ----------------------------------------------------------------------
# Public selector (platform-aware)
# ----------------------------------------------------------------------
alias(
    name = "pywrap_litert_binaries",
    actual = select({
        "@bazel_tools//src/conditions:windows": "//litert/python/impl_windows:pywrap_litert_binaries_windows",
        "//conditions:default": ":pywrap_litert_binaries_unix",
    }),
    visibility = ["//visibility:public"],
)

