load("@rules_cc//cc:defs.bzl", "cc_binary", "cc_library", "cc_test")
load("@rules_pkg//pkg:install.bzl", "pkg_install")
load("@rules_pkg//pkg:mappings.bzl", "pkg_attributes", "pkg_filegroup", "pkg_files", "strip_prefix")

package(default_visibility = ["//visibility:public"])

pkg_files(
    name = "package_files_libraries",
    srcs = [":lane_follow_planner"],
    attributes = pkg_attributes(mode = "0644"),
    prefix = "local_planning/lib",
    strip_prefix = strip_prefix.from_root("modules/local_planning"),
)

pkg_filegroup(
    name = "package_files",
    srcs = [":package_files_libraries"],
)

pkg_install(
    name = "install_dir",
    srcs = [":package_files"],
    tags = ["manual"],
)

cc_library(
    name = "scenario",
    srcs = ["scenario.cc"],
    hdrs = ["scenario.h"],
    deps = [
        "//modules/local_planning/planning_context:input_gate",
        "@wheelos_common//modules/common/status",
    ],
)

cc_test(
    name = "local_planning_core_test",
    size = "small",
    srcs = ["local_planning_core_test.cc"],
    deps = [
        ":scenario",
        "//modules/local_planning/planning_context:input_gate",
        "@com_google_googletest//:gtest_main",
        "@wheelos_common//modules/common/status",
    ],
)

cc_library(
    name = "lane_follow_planner",
    srcs = ["lane_follow_planner.cc"],
    hdrs = ["lane_follow_planner.h"],
    deps = [
        ":scenario",
        "//modules/local_planning/planning_context:environment_geometry",
        "//modules/local_planning/planning_context:planning_input_builder",
        "//modules/local_planning/planning_context:trajectory_geometry",
        "//modules/local_planning/trajectory_continuity",
        "//modules/local_planning/trajectory_continuity:trajectory_validation",
        "@wheelos_common//modules/common/math:geometry",
        "@wheelos_common//modules/common/status",
    ],
)

cc_library(
    name = "closed_loop",
    srcs = ["simulation/closed_loop.cc"],
    hdrs = ["simulation/closed_loop.h"],
    deps = [
        ":lane_follow_planner",
        "//modules/local_planning/planning_context:input_gate",
        "@wheelos_common//modules/common/status",
        "@wheelos_simulation//modules/simulation:simulation_engine",
        "@wheelos_simulation//modules/simulation:simulation_types",
        "@wheelos_world_model//modules/world_model/local_map:local_scene_builder",
    ],
)

cc_binary(
    name = "local_planning_sim",
    srcs = ["simulation/main.cc"],
    data = ["@wheelos_simulation//modules/simulation:runtime_data"],
    deps = [
        ":closed_loop",
        "@wheelos_common//modules/common/status",
    ],
)

cc_test(
    name = "lane_follow_planner_test",
    size = "small",
    srcs = ["lane_follow_planner_test.cc"],
    deps = [
        ":closed_loop",
        ":lane_follow_planner",
        "//modules/local_planning/planning_context:input_gate",
        "@com_google_googletest//:gtest_main",
        "@wheelos_common//modules/common/status",
    ],
)

cc_test(
    name = "closed_loop_test",
    size = "medium",
    srcs = ["simulation/closed_loop_test.cc"],
    data = ["@wheelos_simulation//modules/simulation:runtime_data"],
    deps = [
        ":closed_loop",
        "@com_google_googletest//:gtest_main",
        "@wheelos_common//modules/common/status",
    ],
)

cc_test(
    name = "framework_test",
    size = "small",
    srcs = ["framework_test.cc"],
    deps = [
        ":closed_loop",
        ":lane_follow_planner",
        "//modules/local_planning/planning_context:corridor_selector",
        "//modules/local_planning/planning_context:environment_geometry",
        "//modules/local_planning/planning_context:planning_geometry",
        "//modules/local_planning/planning_context:reference_line",
        "//modules/local_planning/planning_context:trajectory_geometry",
        "//modules/local_planning/trajectory_continuity:trajectory_validation",
        "@com_google_googletest//:gtest_main",
        "@wheelos_world_model//modules/world_model/local_map:local_scene_builder",
        "@wheelos_world_model//modules/world_model/local_map:navigation_prior",
    ],
)
