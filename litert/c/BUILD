# Copyright 2024 Google LLC.
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

load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("@rules_cc//cc:cc_shared_library.bzl", "cc_shared_library")
load("@rules_cc//cc:cc_test.bzl", "cc_test")
load(
    "//litert/build_common:litert_build_defs.bzl",
    "export_lrt_runtime_only_linkopt",
    "export_lrt_runtime_only_script",
    "export_lrt_tflite_runtime_linkopt",
    "export_lrt_tflite_runtime_script",
)
load("//litert/build_common:special_rule.bzl", "gles_deps", "gles_headers", "gles_linkopts", "litert_metal_linkopts", "litert_metal_opts")

package(default_visibility = ["//visibility:public"])

# --- Core libraries ---
cc_library(
    name = "litert_common",
    srcs = ["litert_common.cc"],
    hdrs = ["litert_common.h"],
)

cc_library(
    name = "litert_custom_op_kernel",
    srcs = [],  # header-only
    hdrs = ["litert_custom_op_kernel.h"],
    deps = [":litert_common", ":litert_layout"],
)

cc_library(
    name = "litert_any",
    hdrs = ["litert_any.h"],
)

cc_library(
    name = "litert_metrics",
    srcs = ["litert_metrics.cc"],
    hdrs = ["litert_metrics.h"],
    deps = [":litert_any", ":litert_common", "//litert/runtime:metrics"],
    alwayslink = 1,
)

cc_library(
    name = "litert_environment",
    srcs = ["litert_environment.cc"],
    hdrs = ["litert_environment.h"],
    deps = [
        ":litert_common",
        ":litert_environment_options",
        "//litert/cc:litert_macros",
        "//litert/core:environment",
        "//litert/runtime:gpu_environment",
        "//litert/runtime/accelerators:auto_registration",
        "@com_google_absl//absl/types:span",
    ],
    alwayslink = 1,
)

cc_library(
    name = "litert_environment_options",
    srcs = ["litert_environment_options.cc"],
    hdrs = ["litert_environment_options.h"],
    deps = [":litert_any", ":litert_common", "//litert/cc:litert_macros", "//litert/core:environment_options"],
)

cc_library(
    name = "litert_environment_options_header",
    hdrs = ["litert_environment_options.h"],
    tags = ["avoid_dep"],
    deps = [":litert_any", ":litert_common"],
)

cc_library(
    name = "litert_event_type",
    hdrs = ["litert_event_type.h"],
    tags = ["avoid_dep"],
    deps = [":litert_common"],
)

cc_library(
    name = "litert_model_hdrs",
    hdrs = ["litert_model.h"],
    tags = ["avoid_dep"],
    deps = [":litert_common", ":litert_layout", ":litert_model_types", ":litert_op_code"],
)

cc_library(
    name = "litert_logging",
    srcs = ["litert_logging.cc"],
    hdrs = ["litert_logging.h"],
    deps = [":litert_common", "//tflite:minimal_logging", "//tflite/core/c:private_common", "@com_google_absl//absl/strings:string_view"],
)

cc_library(
    name = "litert_layout",
    srcs = ["litert_layout.cc"],
    hdrs = ["litert_layout.h"],
    deps = [":litert_common"],
)

cc_library(
    name = "litert_model_types",
    hdrs = ["litert_model_types.h"],
    deps = [":litert_common", ":litert_layout", ":litert_op_code"],
)

cc_library(
    name = "litert_model",
    srcs = ["litert_model.cc"],
    hdrs = ["litert_model.h"],
    deps = [
        ":litert_common",
        ":litert_layout",
        ":litert_model_types",
        ":litert_op_code",
        "//litert/cc:litert_buffer_ref",
        "//litert/cc:litert_macros",
        "//litert/core/model",
        "//litert/core/model:model_load",
        "//litert/core/model:model_serialize",
        "//tflite/core/c:private_c_api_types",
        "@com_google_absl//absl/strings:string_view",
    ],
)

cc_library(
    name = "litert_op_code",
    hdrs = ["litert_op_code.h"],
    deps = ["//tflite:builtin_ops"],
)

cc_library(
    name = "litert_op_options",
    srcs = ["litert_op_options.cc"],
    hdrs = ["litert_op_options.h"],
    deps = [
        ":litert_common",
        ":litert_layout",
        ":litert_logging",
        ":litert_op_code",
        "//litert/cc:litert_macros",
        "//litert/core/model",
        "//tflite/c:c_api_types",
        "//tflite/converter/core:model_builder_base",
        "//tflite/schema:schema_fbs",
    ],
)

cc_library(
    name = "litert_tensor_buffer_types",
    hdrs = ["litert_tensor_buffer_types.h"],
    tags = ["avoid_dep"],
    deps = [":litert_common"],
)

cc_library(
    name = "litert_event",
    hdrs = ["litert_event.h"],
    deps = [":litert_common", ":litert_event_type"],
)

cc_library(
    name = "litert_custom_tensor_buffer",
    hdrs = ["litert_custom_tensor_buffer.h"],
    deps = [":litert_common", ":litert_model_types", ":litert_tensor_buffer_types"],
)

cc_library(
    name = "litert_tensor_buffer",
    srcs = ["litert_tensor_buffer.cc", "litert_tensor_buffer_requirements.cc"],
    hdrs = ["litert_tensor_buffer.h", "litert_tensor_buffer_requirements.h"],
    copts = litert_metal_opts(),
    linkopts = gles_linkopts() + litert_metal_linkopts(),
    deps = [
        ":litert_common",
        ":litert_custom_tensor_buffer",
        ":litert_event",
        ":litert_gl_types",
        ":litert_logging",
        ":litert_model_types",
        ":litert_tensor_buffer_types",
        "//litert/cc:litert_expected",
        "//litert/cc:litert_macros",
        "//litert/runtime:tensor_buffer",
        "@com_google_absl//absl/types:span",
        "@opencl_headers",
    ] + gles_deps(),
    alwayslink = 1,
)

cc_library(
    name = "litert_model",
    srcs = ["litert_model.cc"],
    hdrs = ["litert_model.h"],
    deps = [
        ":litert_common",
        ":litert_layout",
        ":litert_model_types",
        ":litert_op_code",
        "//litert/cc:litert_buffer_ref",
        "//litert/cc:litert_macros",
        "//litert/core/model",
        "//litert/core/model:model_load",
        "//litert/core/model:model_serialize",
        "//tflite/core/c:private_c_api_types",
        "@com_google_absl//absl/strings:string_view",
    ],
)

cc_library(
    name = "litert_options",
    srcs = ["litert_options.cc"],
    hdrs = ["litert_options.h"],
    deps = [":litert_common", ":litert_logging", "//litert/cc:litert_handle", "//litert/cc:litert_macros", "//litert/cc:litert_opaque_options", "//litert/core:options"],
)

cc_library(
    name = "litert_compiled_model",
    srcs = ["litert_compiled_model.cc"],
    hdrs = ["litert_compiled_model.h"],
    deps = [
        ":litert_common",
        ":litert_environment",
        ":litert_logging",
        ":litert_metrics",
        ":litert_model",
        ":litert_options",
        ":litert_tensor_buffer",
        "//litert/cc:litert_macros",
        "//litert/runtime:compiled_model",
        "//tflite/c:c_api_types",
        "@com_google_absl//absl/types:span",
    ],
    alwayslink = 1,
)

cc_library(
    name = "litert_opaque_options",
    srcs = ["litert_opaque_options.cc"],
    hdrs = ["litert_opaque_options.h"],
    deps = [":litert_common"],
)

cc_library(
    name = "litert_profiler",
    srcs = ["litert_profiler.cc"],
    hdrs = ["litert_profiler.h"],
    deps = [":litert_common", ":litert_profiler_event", "//litert/cc:litert_macros", "//litert/runtime:profiler"],
)

cc_library(
    name = "litert_profiler_event",
    hdrs = ["litert_profiler_event.h"],
    deps = ["//tflite/core/api", "//tflite/profiling:memory_info"],
)

# --- C API shared library ---
LITERT_C_API_COMMON_DEPS = [
    ":litert_any",
    ":litert_common",
    ":litert_compiled_model",
    ":litert_custom_tensor_buffer",
    ":litert_options",
    ":litert_event",
    ":litert_environment",
    ":litert_layout",
    ":litert_logging",
    ":litert_metrics",
    ":litert_model",
    ":litert_model_types",
    ":litert_op_code",
    ":litert_op_options",
    ":litert_profiler",
    ":litert_tensor_buffer",
]

cc_shared_library(
    name = "litert_runtime_c_api_so",
    additional_linker_inputs = export_lrt_runtime_only_script(),
    features = ["-thin_lto"],
    shared_lib_name = "libLiteRtRuntimeCApi.so",
    user_link_flags = export_lrt_runtime_only_linkopt(),
    deps = LITERT_C_API_COMMON_DEPS,
)

cc_import(
    name = "litert_runtime_c_api_so_or_dylib",
    shared_library = ":litert_runtime_c_api_so",
    hdrs = glob(["*.h"]) + [
        "//litert/c/internal:internal_headers",
        "//litert/c/options:options_headers",
        "//tflite:builtin_ops.h",
        "//tflite/c:tensorflowlite_c_api_hdrs_filegroup",
        "//tflite/core/c:headers_filegroup",
    ],
)

cc_library(
    name = "litert_runtime_c_api_shared_lib",
    hdrs = glob(["*.h"]) + [
        "//litert/c/internal:internal_headers",
        "//litert/c/options:options_headers",
        "//tflite:builtin_ops.h",
        "//tflite/c:tensorflowlite_c_api_hdrs_filegroup",
        "//tflite/core/c:headers_filegroup",
    ],
    linkopts = [],
    linkstatic = 1,
    deps = [
        ":litert_runtime_c_api_so_or_dylib",
        "@com_google_absl//absl/types:span",
        "@opencl_headers",
        "//litert/vendors/c:litert_dispatch_c_api",
        "//tflite/core/api",
        "//tflite/profiling:memory_info",
        "//tflite/profiling:profile_buffer",
    ] + gles_headers(),
)

# --- Tests ---
cc_test(
    name = "litert_common_test",
    srcs = ["litert_common_test.cc"],
    deps = [":litert_common", "@com_google_googletest//:gtest_main"],
)

cc_test(
    name = "litert_environment_options_test",
    srcs = ["litert_environment_options_test.cc"],
    deps = [":litert_any", ":litert_common", ":litert_environment_options", "//litert/core:environment_options", "//litert/test:matchers", "@com_google_googletest//:gtest_main"],
)

cc_test(
    name = "litert_model_test",
    srcs = ["litert_model_test.cc"],
    deps = [":litert_common", ":litert_model", ":litert_op_code", "//litert/cc:litert_buffer_ref", "//litert/core/model", "//litert/core/util:flatbuffer_tools", "//litert/test:matchers", "@com_google_absl//absl/strings:string_view", "@com_google_absl//absl/types:span", "@com_google_googletest//:gtest_main"],
)

cc_test(
    name = "litert_tensor_buffer_test",
    srcs = ["litert_tensor_buffer_test.cc"],
    tags = ["requires-gpu-nvidia"],
    deps = [":litert_any", ":litert_common", ":litert_environment", ":litert_environment_options", ":litert_model", ":litert_tensor_buffer", ":litert_tensor_buffer_types", "//litert/cc:litert_any", "//litert/cc:litert_layout", "//litert/runtime:tensor_buffer", "//litert/test:matchers", "@com_google_googletest//:gtest_main"],
)

exports_files(srcs = glob(["litert_*.h"]))
