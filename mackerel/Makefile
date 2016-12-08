MKR=$(shell which mkr)
HUB=bin/hub
HUB_CONFIG=
dirs=$(HOME)/.config bin tmp
monitors=monitors.json
current=$(shell date "+%Y/%m/%d %H:%M:%S")
pr_branch=update-monitors-json-$(shell date "+%Y%m%d%H%M")
pr_text_file=tmp/pr_text_file

check-mackerel-monitors:
	$(MAKE) diff || $(MAKE) create-pull-request

pull:
	$(MKR) monitors pull --file-path $(monitors)

diff:
	$(MKR) monitors diff -e --file-path $(monitors)

create-pull-request: $(HUB) $(HOME)/.config/hub
	$(MAKE) $(pr_text_file)
	$(MAKE) pull
	$< checkout -b $(pr_branch)
	$< add $(monitors)
	$< commit -m "Update monitors.json $(current). job: $(job_url)"
	$< push origin $(pr_branch)
	$< pull-request -F $(pr_text_file)

$(pr_text_file): tmp
	echo "Update monitors.json $(current)." > $(pr_text_file)
	echo "" >> $(pr_text_file)
	echo "job: $(job_url)" >> $(pr_text_file)
	$(MKR) monitors diff --file-path $(monitors) >> $(pr_text_file)

bin/hub: tmp bin
	wget -O $</hub-linux-amd64-2.2.9.tgz https://github.com/github/hub/releases/download/v2.2.9/hub-linux-amd64-2.2.9.tgz
	cd $< && tar xzvf hub-linux-amd64-2.2.9.tgz
	mv $</hub-linux-amd64-2.2.9/bin/hub $@
	rm -rf $<

$(HOME)/.config/hub: $(HUB_CONFIG) $(HOME)/.config
	cp -f $< $@

$(dirs):
	mkdir -p $@
