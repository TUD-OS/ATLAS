all:: $(addsuffix .log,$(basename $(notdir $(wildcard Samples/*.*))))

WORKBENCH_BASE ?= ../..
include $(WORKBENCH_BASE)/Makefile

%.log: Samples/%.h264_metrics ffplay Makefile
	dd of=/dev/null bs=1024x1024 count=100 < $<  # load first 100MiB into buffer cache
	./ffplay -f h264 -threads 1 -nostats -noframedrop -autoexit $< > $@

%.h264_metrics: force
	$(MAKE) -C ../03\ Preprocess\ Metrics $@

clean::
	rm -f *.log
