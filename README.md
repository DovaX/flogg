# flogg
Improved logging system for Python

## Table of Contents
- [Import](#import)
- [Basic Usage](#basic-usage)
- [Log Levels](#log-levels)
- [Runtime Log Level Configuration](#runtime-log-level-configuration)
- [Using in Classes](#using-in-classes)
- [Using Outside Classes](#using-outside-classes)
- [Message Categories](#message-categories)
- [Available Enums](#available-enums)
- [Configuration](#configuration)
- [Developer Mode](#developer-mode)
- [Examples](#examples)
- [Backward Compatibility](#backward-compatibility)

## Import

### Recommended Usage (flogg)

```python
import flogg
# or
from flogg import FlogLogger, FlogLevel, info, error, warning, debug, etc.
```

### Backward Compatibility (forloop_modules.flog)

```python
# Still works for backward compatibility
import forloop_modules.flog as flog
```

## Basic Usage

### Using flogg (Recommended)

```python
import flogg

flogg.debug("Debug message")
flogg.minor_info("Minor info message")
flogg.info("Info message")
flogg.warning("Warning message")
flogg.error("Error message")
flogg.critical("Critical message")
```

### Using forloop_modules.flog (Backward Compatible)

```python
import forloop_modules.flog as flog

flog.debug("Debug message")
flog.minor_info("Minor info message")
flog.info("Info message")
flog.warning("Warning message")
flog.error("Error message")
flog.critical("Critical message")
```

## Log Levels

Log levels from lowest to highest:

- **DEBUG (10)** - Detailed debugging information
- **MINORINFO (15)** - Minor informational messages
- **INFO (20)** - General informational messages
- **WARNING (30)** - Warning messages (yellow)
- **ERROR (40)** - Error messages (red)
- **CRITICAL (50)** - Critical error messages (red)

## Runtime Log Level Configuration

### Using the Logger Instance (flogg - Recommended)

```python
import flogg

# Set default log level (using enum)
flogg.logger.set_log_level(flogg.FlogLevel.DEBUG)

# Set log level using string (case-insensitive)
flogg.logger.set_log_level("debug")
flogg.logger.set_log_level("INFO")
flogg.logger.set_log_level("Warning")

# Set log level for specific class
flogg.logger.set_log_level(flogg.FlogLevel.INFO, class_name="MyClass")
flogg.logger.set_log_level("info", class_name="MyClass")

# Get current log level
level = flogg.logger.get_log_level()
level = flogg.logger.get_log_level(class_name="MyClass")

# Invalid log level raises ValueError
flogg.logger.set_log_level("invalid")  # Raises ValueError with helpful message
```

### Direct FLOG_CONFIG Manipulation (works in both versions)

```python
import flogg

# Set default log level
flogg.FLOG_CONFIG["DEFAULT"] = flogg.FlogLevel.DEBUG

# Set log level for specific class
flogg.FLOG_CONFIG["MyClass"] = flogg.FlogLevel.INFO

# Get current log level
level = flogg.FLOG_CONFIG.get("DEFAULT", flogg.FlogLevel.WARNING)
```

### Backward Compatible (forloop_modules.flog)

```python
import forloop_modules.flog as flog

# All the same methods work
flog.logger.set_log_level(flog.FlogLevel.DEBUG)
flog.FLOG_CONFIG["DEFAULT"] = flog.FlogLevel.INFO
```

## Using in Classes

The logger automatically detects the class name from which it's called:

```python
import flogg

class MyClass:
    def __init__(self):
        flogg.info("Initializing MyClass")  # Automatically detects class name
    
    def my_method(self):
        flogg.debug("Debug message from method")
```

## Using Outside Classes

When called outside of a class, no class name is shown in the output:

```python
import flogg

flogg.info("This is a standalone message")  # No class name in output
```

## Message Categories

Filter messages by category (default: "*" = all):

```python
import flogg

# Log with a specific category
flogg.info("Message", message_category="important")

# Configure which categories to display
flogg.MESSAGE_CATEGORIES = ["important", "errors"]
```

## Available Enums

### FlogLevel
- `CRITICAL`
- `ERROR`
- `WARNING`
- `INFO`
- `MINORINFO`
- `DEBUG`
- `NOTSET`

### LogColor
- `OKGREEN`
- `ERROR`
- `WARNING`
- `BOLD`
- `COLOROFF`

## Configuration

### FLOG_CONFIG

Dictionary mapping class names to log levels:

- **Default level**: `WARNING`
- **Pre-configured classes**: `Wizard`, `Scanner`, `CleaningUtility`, `DfToListHandler`

Example:

```python
import flogg

flogg.FLOG_CONFIG["DEFAULT"] = flogg.FlogLevel.INFO
flogg.FLOG_CONFIG["MyClass"] = flogg.FlogLevel.DEBUG
```

## Developer Mode

- **When `DEVELOPER_MODE=True`**: Colored output, timestamps, class names
- **When `DEVELOPER_MODE=False`**: Logging is disabled

## Examples

### Basic Logging

```python
import flogg

flogg.info("Application started")
flogg.warning("Low memory detected")
flogg.error("Failed to connect to database")
```

### Runtime Configuration

```python
import flogg

# Enable debug logging
flogg.logger.set_log_level(flogg.FlogLevel.DEBUG)
flogg.debug("This will now be visible")
```

### Class-Specific Configuration

```python
import flogg

# Set debug level for a specific class
flogg.FLOG_CONFIG["DatabaseHandler"] = flogg.FlogLevel.DEBUG
```

### Using in a Class

```python
import flogg

class DeploymentSupervisor:
    def supervise(self):
        flogg.info("Starting supervision")
        flogg.debug("Checking deployments...")
```

## Help Method

You can view help in Python:

```python
# Using flogg (recommended)
import flogg
flogg.help()

# Or using backward compatible import
import forloop_modules.flog as flog
flog.help()
```

## Backward Compatibility

The `forloop_modules.flog` module is maintained for backward compatibility. It imports everything from `flogg`, so all existing code using `import forloop_modules.flog as flog` will continue to work without any changes.

```python
# Old code - still works!
import forloop_modules.flog as flog
flog.info("This still works")
flog.logger.set_log_level(flog.FlogLevel.DEBUG)

# New code - recommended
import flogg
flogg.info("This is the new way")
flogg.logger.set_log_level(flogg.FlogLevel.DEBUG)
```