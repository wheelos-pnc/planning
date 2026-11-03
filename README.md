# WheelOS Planning

Independent `wheelos_planning` capability module containing `planning`,
`local_planning`, `open_space_planning`, and the Apollo `modules/map` adapters,
tools and fixtures.

HDMap loading/query algorithms remain in the independent `wheelos_map` package.
The map source path remains `modules/map/...`; product-wide defaults live in
the product's `conf/`. Map selection and deployment assets are not owned by a
planning algorithm merely because the existing map directory is packaged here.

The source layout, C++ includes and Proto import paths retain `modules/...`.
Components and tests remain individually selectable; sharing a package does
not merge runtime processes or require every GPU model to be built.

Use the mapped non-root managed container. From this checkout, run
`bazelisk cquery --config=dev -c opt //modules/open_space_planning:libopen_space_planning_component.so`; the Apollo product uses
`@wheelos_planning//modules/...` through opt-in development overrides.
Live vehicle/map/model selection belongs to deployment configuration.

SDK installs apply `third_party/rules_pkg/execroot_runfiles.patch`.
Consuming roots must carry that override; dependency-root overrides do not apply.
This ignored checkout must accompany product changes. Its Git source is
registered in `wheelos.repos`; no Bzlmod registry release has been published.

Configured CPU analysis is not GPU inference acceptance. GPU targets
retain their CUDA/TensorRT/model requirements; broad unconfigured
queries may load disabled or legacy backend repositories.
