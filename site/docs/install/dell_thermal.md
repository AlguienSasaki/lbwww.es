---
title: Dell Latitude thermal throttling
x-toc-enable: true
...

Recent Libreboot releases fixed this, and this page is provided for reference.
You can exceed 87C even. Dell implements much more conservative thermal
protection by default, that you can turn off.

The rest of this page pertains only to older Libreboot releases, prior
to Libreboot 26.10rev10. Newer 26.10 revisions and releases after 26.10
fix this issue entirely, but of course you still want to make sure that
you don't reach such high temperatures. Thermal efficiency can be a bit
shoddy on some of these Dell laptops, so do take care:

Basically, what you need to do is:

* Use high quality thermal paste (don't use the same dried up paste that the
  laptop came with, if you bought it on ebay for example). Arctic MX-6 is good.
* Check that the fan works reliably

Also: the `intel_pstate` driver can be used to artificially cap CPU speed. See:

<https://www.kernel.org/doc/html/v4.12/admin-guide/pm/intel_pstate.html>

When you use this machine, it is recommended that you cap the CPU speed once
you've booted into Linux. Set it to something like 50% at first. Then run a
stress test, for example:

	stress -c x

Where `x` is the number of CPU cores, e.g. 2. Monitor the temperatures using
something like `xsensors`, making sure the CPU doesn't exceed 80c temperature.

You can also monitor CPU speeds in Linux like so:

	watch -n .2 grep MHz /proc/cpuinfo

This will let you know what speed you're at. You can use this to determine
whether the `intel_pstate` driver is working. How to cap speed to 50 percent, as
in the above example:

	echo 50 > /sys/devices/system/cpu/cpufreq/intel_pstate/max_perf_pct

Gradually increase the CPU speed (up to 100 on `max_perf_pct`), waiting a few
minutes each time. You should ensure that your machine does not exceed 80C.

Dell's thermal safety is far too protective by default, on some of these, and
we don't yet know how to properly configure it. Running a CPU below 80c in
temperature and never higher than that, is a good idea anyway, for the
long term life of your CPU.

Regardless, thermal shutdown is extremely reliable on this machine, but Dell
makes it shut down *earlier*, before it can even start to CPU throttle.

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/dell_thermal.html>
