# Managing a Telemetry Pipeline: The Build Versus Buy Argument

For historical reasons, Slack uses a mix of open source and in-house software in its telemetry pipeline management. These days, these reasons have less meaning, as a multitude of open source alternatives exist and can be used to easily form the basis of your telemetry pipeline. These open source tools are usually modular in nature and can be easily extended to add additional functionality for your custom use cases. Given the wide availability of tools, it does not make economic sense to spend engineering resources developing in-house software to make up the basis of your telemetry management pipeline.

Open source software, naturally, is free, but the cost of maintaining and operating a telemetry pipeline should be taken into consideration. Vendors exist on the other side of the build-versus-buy equation and promise to simplify the operational burden as well as provide a strong user experience. Simplification and ease of use is an attractive proposition and may make economic sense, depending on the maturity of your organization. This balance will shift over time as your organization grows, so keep that in mind when selecting your approach. For more considerations when deciding whether to build or to buy, see Chapter 15.

# Conclusion

You can get started today by using several off-the-shelf services in conjunction with one another. You may be tempted to write a new system from scratch, but we recommend adapting an open source service. Smaller pipelines can run on autopilot, but as the organization grows, managing a telemetry pipeline becomes a complex endeavor and introduces several challenges.

As you build out your telemetry management system, try to build for the current needs of the business and anticipate— but don’t implement for— new needs. For example, you may want to add compliance features