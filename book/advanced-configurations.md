# Parameters & Configurations

The PX4 platform uses the param subsystem (a flat table of float and int32_t values) and text files (for mixers and startup scripts) to store its configuration.

The [system startup](advanced-system-startup.md) and how [airframe configurations](advanced-adding-a-new-frame.md) work are detailed on other pages. This section discusses the param subsystem in detail

## Commandline usage

The PX4 [system console](advanced-system-console.md) offers the ```param``` tool, which allows to set parameters, read their value, save them and export and restore to and from files.

### Getting and Setting Parameters

The param show command lists all system parameters:

<div class="host-code"></div>

```sh
param show
```

To be more selective a partial parameter name with wildcard can be used:

<div class="host-code"></div>

```sh
nsh> param show RC_MAP_A*
Symbols: x = used, + = saved, * = unsaved
x   RC_MAP_AUX1 [359,498] : 0
x   RC_MAP_AUX2 [360,499] : 0
x   RC_MAP_AUX3 [361,500] : 0
x   RC_MAP_ACRO_SW [375,514] : 0

 723 parameters total, 532 used.
```

### Exporting and Loading Parameters

The standard save command will store the parameters in the current default file:

<div class="host-code"></div>

```sh
param save
```

If provided with an argument, it will store the parameters instead to this new location:

<div class="host-code"></div>

```sh
param save /fs/microsd/vtol_param_backup
```

There are two different commands to load parameters: ```param load``` will load a file and replace the current parameters with what is in this file, resulting in a 1:1 copy of the state that was present when these parameters were stored. ```param import``` is more subtle: It will only change parameter values which have been changed from the default. This is great to e.g. initially calibrate a board (without configuring it further), then importing the calibration data without overwriting the rest of the system configuration.

Overwrite the current parameters:

<div class="host-code"></div>

```sh
param load /fs/microsd/vtol_param_backup
```

Merge the current parameters with stored parameters (stored values which are non-default take precedence):

<div class="host-code"></div>

```sh
param import /fs/microsd/vtol_param_backup
```

## C / C++ API

There is also a C and a separate C++ API which can be used to access parameter values.

<aside class="todo">
Discuss param C / C++ API.
</aside>

<div class="host-code"></div>

```C
int32_t param = 0;
param_get(param_find("PARAM_NAME"), &param);
```

