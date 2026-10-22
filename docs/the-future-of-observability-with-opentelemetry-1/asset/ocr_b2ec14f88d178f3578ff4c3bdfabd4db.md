# Stability and Long-Term Support

OpenTelemetry was designed to allow long-term stability and innovation to occur side by side. In OpenTelemetry, stability guarantees are provided on a signal-by-signal basis. Rather than look at the version number, check the stability rating for the signals you wish to use.

# Signal Lifecycle

Figure 6-1 shows how new signals are added to OpenTelemetry. Experimental signals are still under development. They may change—and break compatibility—at any time. Experimental signals start development as a specification proposal, which is developed alongside a set of prototypes. Once an experimental signal is ready to be used in production, a feature freeze is put on the signal's specification, and beta releases of the new signal are created in multiple languages. Beta releases may not be feature complete, and they may experience breaking changes, but they are considered ready for production feedback from early adopters. Once a signal is considered ready to be declared stable, a release candidate is issued. If the release candidate can remain stable and without issue for a period of time, then both the specification and the beta releases of the signal are declared stable.

Once a signal becomes stable, it falls under OpenTelemetry's long-term support guarantees. OpenTelemetry takes backward compatibility and seamless upgrades very seriously. See the following sections for details.