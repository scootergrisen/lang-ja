MASTER_PO = ja.po

MOFILES = ja.mo \
	  ja.euc-jp.mo \
	  ja.sjis.mo

CHECKFILES = ja.ck \
	     ja.euc-jp.ck \
	     ja.sjis.ck

MSGFMT = OLD_PO_FILE_INPUT=yes msgfmt

VIM = vim

.SUFFIXES: .po .mo .ck

test: check $(MOFILES)

update: ja.sjis.po ja.euc-jp.po

ja.sjis.po: $(MASTER_PO)
	@$(MAKE) sjiscorr
	rm -f $@
	iconv -f utf-8 -t cp932 $< | ./sjiscorr > $@

ja.euc-jp.po: $(MASTER_PO)
	iconv -f utf-8 -t euc-jp $< | \
	  sed -e 's/charset=utf-8/charset=euc-jp/' -e 's/# Original translations/# Generated from ja.po, DO NOT EDIT/' > $@

sjiscorr: sjiscorr.c
	$(CC) -o $@ $<

force: touch
	@$(MAKE) update

touch: $(MASTER_PO)
	touch $<

merge:
	rm -f ja.po.old
	mv ja.po ja.po.old
	msgmerge ja.po.old vim.pot -o ja.po

merge-force:
	touch vim.pot
	@$(MAKE) merge

clean: checkclean
	rm -f sjiscorr sjiscorr.exe
	rm -f ja.sjis.po ja.euc-jp.po
	rm -f *.mo

#ja.po: vim.pot
#	rm -f $@.old
#	mv $@ $@.old
#	msgmerge $@.old $< -o $@

.po.mo:
	$(MSGFMT) --check -v -o $@ $<

.po.ck:
	$(VIM) -u NONE --noplugins -e -s -X --cmd "set enc=utf-8" -S check.vim \
	  -c "if error == 0 | q | else | num 2 | cq | endif" $<
	touch $@

check: $(CHECKFILES)

checkclean:
	rm -f *.ck
