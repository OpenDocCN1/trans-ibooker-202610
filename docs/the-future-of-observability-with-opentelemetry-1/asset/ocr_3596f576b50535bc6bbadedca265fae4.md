owners and library developers should never have to re-instrument their application in order to upgrade to a new version of the API.

If an OpenTelemetry API were to be deprecated, which is unlikely, the deprecated API would still remain stable and functional.

# Native Instrumentation is Stable Instrumentation

OSS libraries that ship with native OpenTelemetry instrumentation should only use stable APIs, as changes to experimental features may create a dependency conflict.

That said, we definitely encourage beta testing. If a library would like to offer support for experimental OpenTelemetry features, that is a great way to give the project feedback and participate in the design of new features. However, we recommend that integration with experimental OpenTelemetry features be offered as optional plug-ins, which the end user must install separately to enable.

Once features become stable, there is no need to keep them as a separate plug-in. In fact, it is better to integrate OpenTelemetry natively, as users may forget to install plug-ins. That way, if the application owner installs the OpenTelemetry SDK, they automatically start receiving data from every library. If no SDK is installed, the API calls are no-ops. Native instrumentation is one way we hope that OpenTelemetry will simplify observability for application owners—it’s already present in every library and ready to go as soon as it’s wanted.

# SDK and Collector Stability

SDK stability focuses on two areas: plug-in interfaces and resource usage. The SDK may occasionally deprecate a plug-in interface. To ensure that application owners can upgrade cleanly, a replacement interface must be added before the deprecated interface is removed, and popular plug-ins that use the deprecated interface must be migrated to the new interface. By safely migrating the plug-in ecosystem in this manner, we avoid catching application owners in a situation where they want to upgrade, but are blocked by an incompatible plug-in. There must be at least a six-month window between deprecating and removing a plug-in interface, and deprecated interfaces should only be slated for removal when maintaining them