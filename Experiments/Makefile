.PHONY: all clean cleanall environment

all: environment

all clean:
	$(MAKE) -C 01\ Decoding\ Times $@
	$(MAKE) -C 02\ Times\ and\ Metrics $@
	$(MAKE) -C 03\ Preprocess\ Metrics $@
	$(MAKE) -C 04\ GCD\ Compatibility $@
	$(MAKE) -C 05\ Prediction $@
	$(MAKE) -C 06\ Scheduling $@

%:
	for subdir in $@* ; do $(MAKE) -C "$$subdir" ; done

# suggestion: to prevent the sudo prompt, put this in /etc/sudoers.d/
# $USER ALL = NOPASSWD: /bin/dd of=/proc/sys/kernel/sched_atlas_advance_in_cfs
# $USER ALL = NOPASSWD: /sbin/modprobe msr
# $USER ALL = NOPASSWD: /usr/sbin/rdmsr
# $USER ALL = NOPASSWD: /usr/sbin/wrmsr

environment:
	@uname -sr | grep -q 'Linux.*atlas' || { echo '*** Boot the ATLAS kernel. ***' ; false ; }
	@fgrep -q 1 /proc/sys/kernel/sched_atlas_advance_in_cfs || echo -n 1 | sudo dd of=/proc/sys/kernel/sched_atlas_advance_in_cfs
	@fgrep -q performance /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor || { echo '*** Use the "performance" CPU frequency governor. ***' ; false ; }
	@lsmod | fgrep -q msr || sudo modprobe msr
	@sudo rdmsr 0x1a0 --bitfield 38:38 | fgrep -q 1 || sudo wrmsr 0x1a0 $$((`sudo rdmsr 0x1a0 --decimal` | 0x4000000000))  # disable Intel turbo mode
	@! grep -qv 'lo$$' /proc/net/if_inet6 || { echo '*** Disable networking. ***' ; false ; }
	@! hciconfig | fgrep -q UP || { echo '*** Disable Bluetooth. ***' ; false ; }
	@! ps -eo pid,rtprio,cmd | fgrep -v fgrep | fgrep ' - /usr/bin/X ' || { echo '*** Boost X to real-time priority (chrt -p 5 <PID>). ***' ; false ; }

cleanall: clean
