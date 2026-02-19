# Debugging Torch-Spyre
This document provides an overview of various tools or environment variables that can be used for debugging while working with Torch-Spyre.

## Enable Debug Logging

Debug logging is not enabled by default. Set `TORCH_SPYRE_DEBUG=1` to enable debug logging.
This will output debug logs with function calls, tensor operations, memory allocations, etc.

`TORCH_SPYRE_DEBUG` supports following values:
- Default value: 0
- Accepted value: 0, 1

## Disable Downcast Warnings

The warnings related to potential precision loss (e.g., Backend Spyre does not support int64; downcasting to int32) are useful for debugging and
enabled by default via the `TORCH_SPYRE_DOWNCAST_WARN` environment variable. However, when you intend to use lower precision, you may want to
disable related warnings to reduce clutter in the log. You can do so by setting it to 0 or set it to false or off to disable downcast warning.

`TORCH_SPYRE_DOWNCAST_WARN` supports following values:
- Default value: 1
- Accepted value: 0/1, true/false, on/off

## Logging Verbosity

The following flags are useful for compiler and runtime debugging. By default, they are set to minimal logging to reduce clutter. Change the default setting as you need.

- `DTLOG_LEVEL` - an environment variable that controls the logging level for the Spyre software stack. To keep the logging in a quiet mode,
by default it is set to `error` level. If you want to see more detailed logging, change the logging level to warning, info, or debug.
  - Default value: error
  - Accepted value: error, warning, info, debug

- `DT_DEEPRT_VERBOSE` - an environment variable that controls the verbosity level of runtime logging. By default, it is enabled for minimal logging to reduce cluttering.
  - Default value: -1

- `TORCH_SENDNN_LOG` - an environment variable related to the runtime logging level for the torch_sendnn library of the Spyre software stack.
To keep logging in quiet mode, it is set to the CRITICAL level by default.
  - Default value: CRITICAL

## Torch-Spyre Inductor specific Logging
Use the following environment variables for the Torch-Spyre specific Inductor module level logging.

- `SPYRE_INDUCTOR_LOG` - enable or disable logging using this environment variable. By default, it is not enabled with the value set to '0'. Set it to `1` to enable.
  - Default value: 0
  - Accepted value: 0, 1

- `SPYRE_INDUCTOR_LOG_LEVEL` - an environment variable that allows you to customize the verbosity when SPYRE_INDUCTOR_LOG is enabled (i.e., `SPYRE_INDUCTOR_LOG=1`). The default log level is set to `Info`. As per your preference, you can change it to error, warning, or debug level.
  - Default value: Info
  - Accepted value: error, warning, info, debug

- `SPYRE_LOG_FILE` - use this environment variable to configure the logging location. If set with a file path, the log will be directed to the specified file. If not set, the log will be streamed to stderr.
  - Default value: stederr

## PyTorch Compiler Logging

PyTorch provides many tools and flags to help with debugging. For example, you can use the `TORCH_LOGS` environment variable to selectively
enable parts of the `torch.compile` stack to the log. The `TORCH_LOGS` variable can take several different options as a value. An example is provided below, which can
output logging from PyTorch Dynamo and Inductor components:

``` python3
TORCH_LOGS="+dynamo,+inductor" python3 examples/softmax.py
```

Refer to the [PyTorch Compiler specific logging](https://docs.pytorch.org/docs/stable/user_guide/torch_compiler/torch.compiler_troubleshooting.html) for more information.

## Example commands

``` python3
TORCH_SPYRE_DEBUG=1 python3 examples/softmax.py
```

``` python3
export TORCH_SPYRE_DEBUG=1
export TORCH_LOGS="+inductor"
export DTLOG_LEVEL=error
python3 examples/softmax.py
```
