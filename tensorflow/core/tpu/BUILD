# Description: Utilities for TPU Operations

load(
    "//tensorflow:tensorflow.bzl",
    "if_windows",
)

package(
    default_visibility = [
        "//tensorflow/core/tpu:__subpackages__",
        "//tensorflow/stream_executor/tpu:__subpackages__",
    ],
    licenses = ["notice"],  # Apache 2.0
)

cc_library(
    name = "libtftpu_header",
    hdrs = ["libtftpu.h"],
    visibility = ["//visibility:public"],
    deps = [],
)

cc_library(
    name = "tpu_embedding_optimization_parameters_utils",
    srcs = ["tpu_embedding_optimization_parameters_utils.cc"],
    hdrs = ["tpu_embedding_optimization_parameters_utils.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core:lib_proto_parsing",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/protobuf/tpu:optimization_parameters_proto_cc",
        "@com_google_absl//absl/base",
    ],
)

cc_library(
    name = "tpu_embedding_output_layout_utils",
    srcs = ["tpu_embedding_output_layout_utils.cc"],
    hdrs = ["tpu_embedding_output_layout_utils.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//tensorflow/core:framework_headers_lib",
        "//tensorflow/core:lib_proto_parsing",
        "//tensorflow/core:protos_all_cc",
        "//tensorflow/core/protobuf/tpu:tpu_embedding_configuration_proto_cc",
        "//tensorflow/core/protobuf/tpu:tpu_embedding_output_layout_proto_cc",
    ],
)

cc_library(
    name = "tpu_compilation_device",
    srcs = ["tpu_compilation_device.cc"],
    visibility = ["//visibility:public"],
    deps = [
        ":tpu_defs",
        ":tpu_node_device_util",
        "//tensorflow/compiler/tf2xla:xla_compiler",
    ],
    alwayslink = 1,
)

cc_library(
    name = "tpu_node_device_util",
    srcs = ["tpu_node_device_util.cc"],
    hdrs = ["tpu_node_device_util.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//tensorflow/compiler/tf2xla:tf2xla_util",
        "//tensorflow/core:lib",
        "//tensorflow/core:protos_all_cc",
    ],
)

cc_library(
    name = "tpu_compile_interface",
    srcs = ["tpu_compile_interface.cc"],
    hdrs = ["tpu_compile_interface.h"],
    deps = [
        "//tensorflow/core/platform:fingerprint",
        "//tensorflow/core/platform:logging",
        "@com_google_absl//absl/strings",
    ],
)

cc_library(
    name = "tpu_defs",
    srcs = ["tpu_defs.cc"],
    hdrs = ["tpu_defs.h"],
    deps = ["//tensorflow/core:protos_all_cc"],
)

cc_library(
    name = "tpu_configuration",
    srcs = ["tpu_configuration.cc"],
    hdrs = ["tpu_configuration.h"],
    deps = ["//tensorflow/core:framework"],
)

cc_library(
    name = "tpu_init_mode",
    srcs = ["tpu_init_mode.cc"],
    hdrs = ["tpu_init_mode.h"],
    deps = [
        "//tensorflow/core:lib",
    ],
)

cc_library(
    name = "tpu_config_c_api",
    hdrs = ["tpu_config_c_api.h"],
    deps = [
        ":libtftpu_header",
        "//tensorflow/c:tf_status",
    ],
    alwayslink = True,
)

cc_library(
    name = "tpu_api",
    srcs = ["tpu_api.cc"],
    hdrs = ["tpu_api.h"],
    deps = [
        ":libtftpu_header",
        ":tpu_config_c_api",
        "//tensorflow/core/tpu/kernels:tpu_compile_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_execute_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_mesh_state_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_util_c_api_hdrs",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
        "//tensorflow/stream_executor/tpu:tpu_node_context_c_api_hdrs",
    ],
)

cc_library(
    name = "tpu_api_dlsym_initializer",
    srcs = if_windows(
        ["tpu_api_dlsym_initializer_windows.cc"],
        otherwise = ["tpu_api_dlsym_initializer.cc"],
    ),
    hdrs = ["tpu_api_dlsym_initializer.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":libtftpu_header",
        ":tpu_api",
        ":tpu_config_c_api",
        ":tpu_library_init_fns",
        "//tensorflow/core/platform:errors",
        "//tensorflow/core/platform:status",
        "//tensorflow/core/tpu/graph_rewrite:tpu_rewrite_pass_registration",
        "//tensorflow/core/tpu/kernels:tpu_compile_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_execute_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_mesh_state_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_util_c_api_hdrs",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
        "//tensorflow/stream_executor/tpu:tpu_node_context_c_api_hdrs",
    ] + select({
        "//tensorflow:oss": [
            ":tpu_compilation_device",
            ":tpu_node_device",
            ":tpu_system_device",
            "//tensorflow/core/tpu/ops:host_compute_ops",
            "//tensorflow/core/tpu/ops:topk_ops",
            "//tensorflow/core/tpu/ops:tpu_compile_op",
            "//tensorflow/core/tpu/ops:tpu_execute_op",
            "//tensorflow/core/tpu/ops:tpu_partitioned_ops",
            "//tensorflow/stream_executor/tpu:tpu_executor",
            "//tensorflow/stream_executor/tpu:tpu_transfer_manager",
            "//tensorflow/core/tpu:tpu_on_demand_compiler",
        ],
        "//conditions:default": [],
    }),
)

cc_library(
    name = "tpu_library_init_fns",
    hdrs = ["tpu_library_init_fns.inc"],
    visibility = ["//visibility:public"],
)

cc_library(
    name = "tpu_node_device",
    srcs = ["tpu_node_device.cc"],
    hdrs = ["tpu_node_device.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":tpu_api",
        ":tpu_defs",
        ":tpu_node_device_util",
        "//tensorflow/compiler/jit:xla_device",
        "//tensorflow/compiler/jit/kernels:xla_ops",
        "//tensorflow/compiler/tf2xla:common",
        "//tensorflow/compiler/tf2xla:tf2xla_util",
        "//tensorflow/compiler/tf2xla:xla_compiler",
        "//tensorflow/core:framework_internal",
        "//tensorflow/core:session_options",
        "//tensorflow/core/common_runtime:copy_tensor",
        "//tensorflow/core/common_runtime:device",
        "//tensorflow/core/common_runtime:device_factory",
        "//tensorflow/core/common_runtime:dma_helper",
        "//tensorflow/core/framework:kernel_def_proto_cc",
        "//tensorflow/core/lib/core:status",
        "//tensorflow/core/platform:status",
        "//tensorflow/core/tpu/kernels:tpu_configuration_ops",
        "//tensorflow/core/tpu/kernels:tpu_util",
        "//tensorflow/stream_executor/tpu:c_api_conversions",
        "//tensorflow/stream_executor/tpu:status_helper",
        "//tensorflow/stream_executor/tpu:tpu_node_context",
        "//tensorflow/stream_executor/tpu:tpu_platform_interface",
        "//tensorflow/stream_executor/tpu:tpu_stream_interface",
    ],
)

cc_library(
    name = "tpu_system_device",
    srcs = ["tpu_system_device.cc"],
    hdrs = ["tpu_system_device.h"],
    visibility = ["//visibility:public"],
    deps = [
        ":virtual_device",
        "//tensorflow/core:framework",
        "//tensorflow/core:graph",
        "//tensorflow/core:session_options",
        "//tensorflow/core/common_runtime:device_factory",
        "//tensorflow/core/lib/core:status",
        "//tensorflow/stream_executor/tpu:tpu_executor_base",
    ],
)

cc_library(
    name = "virtual_device",
    srcs = ["virtual_device.cc"],
    hdrs = ["virtual_device.h"],
    visibility = ["//visibility:public"],
    deps = [
        "//tensorflow/core:core_cpu",
        "//tensorflow/core:protos_all_cc",
    ],
)

cc_library(
    name = "tpu_execute",
    srcs = ["tpu_execute.cc"],
    hdrs = ["tpu_execute.h"],
    deps = [
        ":tpu_api",
        "//tensorflow/compiler/jit:xla_device",
        "//tensorflow/compiler/xla:executable_run_options",
        "//tensorflow/compiler/xla:shape_layout",
        "//tensorflow/compiler/xla:shape_util",
        "//tensorflow/compiler/xla:status",
        "//tensorflow/compiler/xla:status_macros",
        "//tensorflow/compiler/xla:statusor",
        "//tensorflow/compiler/xla:util",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/service:computation_layout",
        "//tensorflow/compiler/xla/service:computation_placer",
        "//tensorflow/compiler/xla/service:executable",
        "//tensorflow/compiler/xla/service:hlo",
        "//tensorflow/compiler/xla/service:hlo_module_config",
        "//tensorflow/compiler/xla/service:hlo_proto_cc",
        "//tensorflow/compiler/xla/service:maybe_owning_device_memory",
        "//tensorflow/compiler/xla/service:transfer_manager",
        "//tensorflow/core:framework",
        "//tensorflow/core:lib",
        "//tensorflow/core/profiler/lib:traceme",
        "//tensorflow/core/tpu/kernels:tpu_compile_c_api_hdrs",
        "//tensorflow/core/tpu/kernels:tpu_executable_info_proto_cc",
        "//tensorflow/stream_executor:device_memory",
        "//tensorflow/stream_executor:stream",
        "//tensorflow/stream_executor/lib",
        "//tensorflow/stream_executor/tpu:c_api_conversions",
        "//tensorflow/stream_executor/tpu:status_helper",
        "//tensorflow/stream_executor/tpu:tpu_executable",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
        "//tensorflow/stream_executor/tpu:tpu_node_context",
        "//tensorflow/stream_executor/tpu:tpu_platform_interface",
        "@com_google_absl//absl/base",
        "@com_google_absl//absl/memory",
    ],
)

cc_library(
    name = "tpu_on_demand_compiler",
    srcs = ["tpu_on_demand_compiler.cc"],
    deps = [
        "//tensorflow/compiler/xla:shape_util",
        "//tensorflow/compiler/xla:xla_data_proto_cc",
        "//tensorflow/compiler/xla/service:compiler",
        "//tensorflow/compiler/xla/service:executable",
        "//tensorflow/compiler/xla/service:hlo",
        "//tensorflow/compiler/xla/service:hlo_cost_analysis",
        "//tensorflow/compiler/xla/service:hlo_module_group",
        "//tensorflow/compiler/xla/service:shaped_buffer",
        "//tensorflow/stream_executor:device_memory_allocator",
        "//tensorflow/stream_executor/tpu:c_api_conversions",
        "//tensorflow/stream_executor/tpu:proto_helper",
        "//tensorflow/stream_executor/tpu:status_helper",
        "//tensorflow/stream_executor/tpu:tpu_executor",
        "//tensorflow/stream_executor/tpu:tpu_executor_c_api_hdrs",
        "@com_google_absl//absl/types:span",
    ],
    alwayslink = True,
)
