# Cloudcraft: Minecraft Cloud Server
__by Liquid Labs__

A robust Google Cloud minecraft server.

## Install

1. [Install terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) if not installed (check with `terraform --version`).
2. [Install gcloud CLI](https://cloud.google.com/sdk/docs/install) if not installed (check with `gcloud --version`).
3. Install Cloudcraft:
   ```bash
   npm i -g cloudcraft
   ```

## Usage

1. Authenticate to gcloud:
   ```bash
   gcloud auth application-default login
   ```
2. Create and start a server:
   ```bash
   cloudcraft server create -- name='smith-family' start
   ```
3. 

_*Untested*_
```bash
npm i liquid-labs/cloudcraft
make deploy
```

_*Prerequisites:*_
- `terraform` cli
- `gcloud` cli
- user has authenticated to the target account where the cloudcraft project will be created

## Features

- Terraform spec uses YAML (which the `make` converts to JSON prior to usage).
- Minecraft service is self-healing (i.e., will restart if it dies using systemd).

## Status

Proof of concept complete.

Next: separate data and runtime; make boot disk entirely ephemeral and use persistent disk for minecraft data only.

See [Minecraft Implementation Diary](https://docs.google.com/document/d/1k8WT486i0k_5MLPrGlIw9xyIHZHS5ZD2kzEFAFv7W_o/edit#) for additional info.

## Command reference

### Usage

`cloudcraft <options> <command>`

### Main options

|Option|Description|
|------|------|
|`<command>`|(_main argument_,_optional_) undefined|
|`--throw-error`|In the case of an exception, the default is to print the message. When --throw-error is set, the exception is left uncaught.|

### Commands

- [`create`](#cloudcraft-create): Creates (sets up) a cloud-based Minecraft server managed by Cloudcraft.
- [`help`](#cloudcraft-help): With no command specified, prints a list of available commands or, when a command is specified, prints help for the specified command.
- [`info`](#cloudcraft-info): Prints info about the Minecraft server(s).
- [`list`](#cloudcraft-list): Lists Cloudcraft managed Minecraft servers.
- [`ssh`](#cloudcraft-ssh): Displays the proper SSH command or executes specified command on remote server.
- [`start`](#cloudcraft-start): Starts the named Minecraft server.
- [`status`](#cloudcraft-status): Displays the status of a Minecraft server.
- [`stop`](#cloudcraft-stop): Stops the named Minecraft server.

<span id="cloudcraft-create"></span>
#### `cloudcraft create <options> [server-name]`

Creates a server named _server-name_. This is, by default, a 'bedrock' server.

##### `create` options

|Option|Description|
|------|------|
|`[server-name]`|(_main argument_,_required_) The name of the server to create.|
|`--server-type`|May be one of: bedrock, java|

<span id="cloudcraft-help"></span>
#### `cloudcraft help <command>`

With no command specified, prints a list of available commands or, when a command is specified, prints help for the specified command.

##### `help` options

|Option|Description|
|------|------|
|`<command>`|(_main argument_,_optional_) The command to print help for.|

<span id="cloudcraft-info"></span>
#### `cloudcraft info <options> <server-name>`

Displays info about the servers or, if _name_ supplied, a server. By default will display all information. If one or more info select options is provided, then it will only display that information.

##### `info` options

|Option|Description|
|------|------|
|`<server-name>`|(_main argument_,_optional_) The name of the server to get into on.|
|`--ip-address`|Select the public IP address for display.|
|`--machine-type`|Select the machine type for display.|
|`--refresh`|Updates underlying terraform files and applies the results before reading the output.|

<span id="cloudcraft-list"></span>
#### `cloudcraft list`

Lists Cloudcraft managed Minecraft servers.

<span id="cloudcraft-ssh"></span>
#### `cloudcraft ssh <options> [server-name]`

The 'ssh' command, with no options, is used to generate and display the SSH command you would use to log into the specified server. With '--eval-mode', you can do:
```
eval $(cloudcraft ssh --eval-mode some-sever
```
You can also use the `--command` option to execute a single command.


##### `ssh` options

|Option|Description|
|------|------|
|`[server-name]`|(_main argument_,_required_) The name of the server to log into.|
|`--command`|Will run the specified command and exit.|
|`--eval-mode`|Produces output suitable to 'eval $(cloudcraft ssh a-server)'.|

<span id="cloudcraft-start"></span>
#### `cloudcraft start <options> [server-name]`

Starts the named Minecraft server.

##### `start` options

|Option|Description|
|------|------|
|`[server-name]`|(_main argument_,_required_) The name of the server to start.|
|`--refresh`|Updates and applies the terraform configuration before starting the server.|

<span id="cloudcraft-status"></span>
#### `cloudcraft status <options> <server-name>`

Tries to determine the if the server is up, it's ping status, and disk usage.

##### `status` options

|Option|Description|
|------|------|
|`<server-name>`|(_main argument_,_optional_) The name of the server to describe.|
|`--no-ping`|Skips the ping test when set.|
|`--refresh`|Updates and applies the terraform configuration before resolving the server status.|

<span id="cloudcraft-stop"></span>
#### `cloudcraft stop <options> [server-name]`

Stops the named Minecraft server.

##### `stop` options

|Option|Description|
|------|------|
|`[server-name]`|(_main argument_,_required_) The name of the server to stop.|
|`--refresh`|Updates and applies the terraform configuration before stoping the server.|



