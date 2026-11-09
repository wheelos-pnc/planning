load("@core//runtime:runtime.bzl", "runtime_module")
load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library")
load("@rules_pkg//pkg:install.bzl", "pkg_install")
load("@rules_pkg//pkg:mappings.bzl", "pkg_attributes", "pkg_filegroup", "pkg_files", "strip_prefix")

package(default_visibility = ["//visibility:public"])

cc_library(
    name = "open_space_planning_component_lib",
    srcs = ["open_space_planning_component.cc"],
    hdrs = ["open_space_planning_component.h"],
    copts = [
        "-DMODULE_NAME=\\\"open_space_planning\\\"",
    ],
    deps = [
        "//modules/open_space_planning/common:types",
        "//modules/open_space_planning/proto:open_space_planning_cc_proto",
        "//modules/open_space_planning/route:skeleton_corridor_route_planner",
        "//modules/open_space_planning/runtime:open_space_planner",
        "//modules/open_space_planning/safety:default_fallback_planner",
        "//modules/open_space_planning/safety:default_trajectory_validator",
        "//modules/open_space_planning/trajectory:open_space_trajectory_planner",
        "@core//cyber",
        "@wheelos_common//modules/common/adapters:adapter_gflags",
        "@wheelos_common//modules/common/vehicle_state:vehicle_state_provider",
        "@wheelos_msgs//wheelos_msgs/chassis_msgs:chassis_cc_proto",
        "@wheelos_msgs//wheelos_msgs/localization_msgs:localization_cc_proto",
        "@wheelos_msgs//wheelos_msgs/planning_msgs:planning_cc_proto",
        "@wheelos_msgs//wheelos_msgs/prediction_msgs:prediction_obstacle_cc_proto",
        "@wheelos_msgs//wheelos_msgs/routing_msgs:routing_cc_proto",
    ],
    alwayslink = True,
)

cc_binary(
    name = "libopen_space_planning_component.so",
    linkshared = True,
    linkstatic = False,
    deps = [":open_space_planning_component_lib"],
)

filegroup(
    name = "runtime_data",
    srcs = [
        "dag/open_space_planning.dag",
        "launch/open_space_planning.launch",
    ],
)

pkg_files(
    name = "package_files_data",
    srcs = [":runtime_data"],
    attributes = pkg_attributes(mode = "0644"),
    prefix = "open_space_planning",
    strip_prefix = strip_prefix.from_root("modules/open_space_planning"),
)

pkg_files(
    name = "package_files_libraries",
    srcs = [":libopen_space_planning_component.so"],
    attributes = pkg_attributes(mode = "0755"),
    prefix = "open_space_planning/lib",
    strip_prefix = strip_prefix.from_root("modules/open_space_planning"),
)

pkg_filegroup(
    name = "package_files",
    srcs = [
        ":package_files_data",
        ":package_files_libraries",
    ],
)

pkg_install(
    name = "install_dir",
    srcs = [":package_files"],
    tags = ["manual"],
)

runtime_module(
    name = "module",
    components = [":libopen_space_planning_component.so"],
    files = {
        "//modules/open_space_planning/runtime:open_space_planning.dag": "dag/open_space_planning/open_space_planning.dag",
        "//modules/open_space_planning/runtime:open_space_planning.launch": "launch/open_space_planning/open_space_planning.launch",
    },
    module_name = "open_space_planning",
)
