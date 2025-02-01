# Python Style Guide

https://google.github.io/styleguide/pyguide.html

## function document example
```python
def get_cpu_info() -> dict:
    """Get information about the CPU through the command ``lscpu``.

    Returns:
        dict: Dictionary containing CPU information with the following keys:
            .. highlight:: python
            .. code-block:: python

                {
                   "socket_number": 1,  # Number of CPU sockets
                   "core_number": 8,  # Number of CPU cores
                   "mhz": 3200.0,  # CPU clock speed in MHz
                   "mhz_max": 3900.0,  # Maximum CPU clock speed in MHz
                   "mhz_min": 800.0,  # Minimum CPU clock speed in MHz
                   "vendor": "GenuineIntel",  # CPU vendor ID
                   "model": "Intel(R) Core(TM) i7-9700K CPU @ 3.60GHz",  # CPU model name
                   "architecture"�?“x86_64�?
               }

    Raises:
        SystemCallError: An error occurs while executing the command to retrieve CPU information.
        SystemCallTimeoutError: A timeout error occurs when executing the command to get CPU information.
    """
```