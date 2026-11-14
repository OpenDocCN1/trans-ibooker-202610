
# Appendix I. Selected .NET and WMI Events

## Table I-1. Selected .NET Events

| Class | Event | Description |
|-------|-------|-------------|
| `System.Diagnostics.​Event⁠ing.Reader.​EventLogWatcher` | `EventRecordWritten` | Allows setting a delegate (event handler method) that gets called every time an event is published that matches the criteria specified in the event query for this object. |
| `System.Data.Common.​DbCon⁠nec⁠tion` | `StateChange` | Occurs when the state of the event changes. |
| `System.Data.SqlClient.​Sql⁠Bulk⁠Copy` | `SqlRowsCopied` | Occurs every time that the number of rows specified by the `NotifyAfter` property have been processed. |
| `System.Data.SqlClient.​Sql⁠Com⁠mand` | `StatementCompleted` | Occurs when the execution of a Transact-SQL statement completes. |
| `System.Data.SqlClient.​Sql⁠Con⁠nection` | `InfoMessage` | Occurs when SQL Server returns a warning or informational message. |
| `System.Data.SqlClient.​Sql⁠Con⁠nection` | `StateChange` | Occurs when the state of the event changes. |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `RowUpdated` | Occurs during `Update` after a command is executed against the data source. The attempt to update is made, so the event fires. |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `RowUpdating` | Occurs during `Update` before a command is executed against the data source. The attempt to update is made, so the event fires. |
| `System.Data.SqlClient.​Sql⁠Data⁠Adapter` | `FillError` | Returned when an error occurs during a fill operation. |
| `System.Data.SqlClient.​Sql⁠De⁠pendency` | `OnChange` | Occurs when a notification is received for any of the commands associated with this `SqlDependency` object. |

## Table I-2. Selected WMI Events

### `__InstanceCreationEvent`

This event class generically represents the creation of instances in WMI providers, such as Processes, Services, Files, and more. A registration for this generic event looks like:

```
$query = "SELECT * FROM __InstanceCreationEvent " +
            "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `__InstanceDeletionEvent`

This event class generically represents the removal of instances in WMI providers, such as Processes, Services, Files, and more. A registration for this generic event looks like:

```
$query = "SELECT * FROM __InstanceDeletionEvent " +
            "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `__InstanceModificationEvent`

This event class generically represents the modification of instances in WMI providers, such as Processes, Services, Files, and more. A registration for this generic event looks like:

```
$query = "SELECT * FROM __InstanceModificationEvent "
            + "WITHIN 5 " +
            "WHERE targetinstance is a
            'Win32_UserAccount'
Register-CimIndicationEvent -Query $query
```

### `Msft_WmiProvider_OperationEvent`

The `Msft_WmiProvider_OperationEvent` event class is the root definition of all WMI provider events. A provider operation is defined as some execution on behalf of a client via WMI that results in one or more calls to a provider executable. The properties of this class define the identity of the provider associated with the operation being executed and is uniquely associated with instances of the class `Msft_Providers`. Internally, WMI can contain any number of objects that refer to a particular instance of `__Win32Provider` since it differentiates each object based on whether the provider supports per-user or per-locale instantiation and also depending on where the provider is being hosted. Currently `TransactionIdentifier` is always an empty string.

### `Win32_ComputerSystemEvent`

This event class represents events related to a computer system.

### `Win32_ComputerShutdownEvent`

This event class represents events when a computer has begun the process of shutting down.

### `Win32_IP4RouteTableEvent`

The `Win32_IP4RouteTableEvent` class represents IP route change events resulting from the addition, removal, or modification of IP routes on the computer system.

### `RegistryEvent`

The registry event classes allow you to subscribe to events that involve changes in hive subtrees, keys, and specific values.

### `RegistryKeyChangeEvent`

The `RegistryKeyChangeEvent` class represents changes to a specific key. The changes apply only to the key, not its subkeys.

### `RegistryTreeChangeEvent`

The `RegistryTreeChangeEvent` class represents changes to a key and its subkeys.

### `RegistryValueChangeEvent`

The `RegistryValueChangeEvent` class represents changes to a single value of a specific key.

### `Win32_SystemTrace`

The `SystemTrace` class is the base class for all system trace events. System trace events are fired by the kernel logger via the event tracing API.

### `Win32_ProcessTrace`

This event is the base event for process events.

### `Win32_ProcessStartTrace`

The `ProcessStartTrace` event class indicates a new process has started.

### `Win32_ProcessStopTrace`

The `ProcessStopTrace` event class indicates a process has terminated.

### `Win32_ModuleTrace`

The `ModuleTrace` event class is the base event for module events.

### `Win32_ModuleLoadTrace`

The `ModuleLoadTrace` event class indicates a process has loaded a new module.

### `Win32_ThreadTrace`

The `ThreadTrace` event class is the base event for thread events.

### `Win32_ThreadStartTrace`

The `ThreadStartTrace` event class indicates a new thread has started.

### `Win32_ThreadStopTrace`

The `ThreadStopTrace` event class indicates a thread has terminated.

### `Win32_PowerManagementEvent`

The `Win32_PowerManagementEvent` class represents power management events resulting from power state changes. These state changes are associated with either the Advanced Power Management (APM) or the Advanced Configuration and Power Interface (ACPI) system management protocols.

### `Win32_DeviceChangeEvent`

The `Win32_DeviceChangeEvent` class represents device change events resulting from the addition, removal, or modification of devices on the computer system. This includes changes in the hardware configuration (docking and undocking), the hardware state, or newly mapped devices (mapping of a network drive). For example, a device has changed when a `WM_DEVICECHANGE` message is sent.

### `Win32_SystemConfigurationChangeEvent`

The `Win32_SystemConfigurationChangeEvent` is an event class that indicates the device list on the system has been refreshed, meaning a device has been added or removed or the configuration changed. This event is fired when the Windows message `DevMgrRefreshOn<ComputerName>` is sent. The exact change to the device list is not contained in the message, and therefore a device refresh is required in order to obtain the current system settings. Examples of configuration changes affected are IRQ settings, COM ports, and BIOS version, to name a few.

### `Win32_VolumeChangeEvent`

The `Win32_VolumeChangeEvent` class represents a local drive event resulting from the addition of a drive letter or mounted drive on the computer system (e.g., CD-ROM). Network drives are not currently supported.

# Appendix J. Standard PowerShell Verbs

Cmdlets and scripts should be named using a `Verb-Noun` syntax—for example, `Get-ChildItem`. The official guidance is that, with rare exception, cmdlets should use the standard PowerShell verbs. They should avoid any synonyms or concepts that can be mapped to the standard. This allows administrators to quickly understand a set of cmdlets that use a new noun.

**Note:** To quickly access this list (without the definitions), type `Get-Verb`.
