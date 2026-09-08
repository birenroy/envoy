Route configurations defer parsing and instantiating dormant VirtualHosts and RouteMatchers until
traffic first matches a domain on worker threads, reducing memory footprint. This behavior can be
reverted by setting the runtime guard ``envoy.reloadable_features.deferred_virtual_host_creation`` to
``false``.
