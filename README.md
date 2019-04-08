# auto-cpu-governor

Automated scripts to change the CPU governor when power state changes between AC and battery

Inspired from a post on [ubuntuforums.org](https://ubuntuforums.org/showthread.php?t=2314674) I wanted to use the `cpupower` utility from linux-tools-generic and add some additional logging to make sure things work as expected.

## External dependencies:

```bash
sudo apt-get install linux-tools-generic
```

## Installation

Copy the udev rules from _udev-rules_:

```bash
sudo cp udev-rules/*.rules /etc/udev/rules.d/
```

Copy the shell scripts from _src_ and make them executable by root:

```bash
sudo cp usr-local-bin/* /usr/local/bin/
sudo chmod 754 /usr/local/bin/cpu-governor-performance
sudo chmod 754 /usr/local/bin/cpu-governor-powersave
```

To see the result right away reload udev rules

```bash
sudo udevadm control --reload
```

Then, enjoy plugging in and removing the power cable while watching the logfile

You may further verify the result by executing `cpupower frequency-info`. Note: this command needs a CPU core index.

A quad-core computer should output something like this:

```bash
$ cpupower frequency-info 0
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 900 MHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 1
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 900 MHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 2
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 850 MHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 3
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "powersave" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 800 MHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

# Plugged in the power cable now

$ cpupower frequency-info 0
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 2.68 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 1
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 2.71 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 2
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 2.72 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes

$ cpupower frequency-info 3
analyzing CPU 0:
  driver: intel_pstate
  CPUs which run at the same hardware frequency: 0
  CPUs which need to have their frequency coordinated by software: 0
  maximum transition latency:  Cannot determine or is not supported.
  hardware limits: 400 MHz - 2.80 GHz
  available cpufreq governors: performance powersave
  current policy: frequency should be within 400 MHz and 2.80 GHz.
                  The governor "performance" may decide which speed to use
                  within this range.
  current CPU frequency: Unable to call hardware
  current CPU frequency: 2.74 GHz (asserted by call to kernel)
  boost state support:
    Supported: yes
    Active: yes
```
