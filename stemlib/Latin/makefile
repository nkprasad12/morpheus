LSDIR=/local/text/ls

all: ends noms vbs deriv

ends: nends vends

nends:
	buildend -L nom
	indendtables -L nom

vends:
	buildend -L verb
	indendtables -L verb

compnoms:
	cat stemsrc/nom.* stemsrc/nom[0-9]* > /usr/tmp/nommorph
	indexcomps < /usr/tmp/nommorph | sort | compcomps > /usr/tmp/nom.heads

deriv:
	buildderiv -L all
	indderivtables -L

noms:stemsrc/nom.irreg stemind/nomind 

vbs: stemind/vbind

stemind/vbind:
	cat stemsrc/vbs.latin.bas stemsrc/vbs.latin > conjfile
	do_conj -L
	mv conjfile.short /usr/tmp/vbmorph
	indexvbs -L

stemsrc/nom.irreg: stemsrc/irreg.nom.src
	buildword -L < stemsrc/irreg.nom.src > stemsrc/nom.irreg

stemsrc/vbs.irreg: stemsrc/irreg.vbs.src
	buildword -L < stemsrc/irreg.vbs.src > stemsrc/vbs.irreg

stemsrc/lemlist:
	cat stemsrc/nom.* stemsrc/nom[0-9]* stemsrc/vbs.irreg stemsrc/vbs.simp.ml stemsrc/vbs.simp.02.new | fgrep ":le:" | cut -d":" -f 3 | sort | awk '{print $$1 " "}' > stemsrc/lemlist
	

stemind/nomind:	
	cat stemsrc/nom.* stemsrc/ls.nom > /usr/tmp/nommorph
	indexnoms -L

ia.noms:
	echo "[aehuwo]i h_hs" > /usr/tmp/skip.ia
	echo ":wd:" >> /usr/tmp/skip.ia
	cat stemsrc/nom0[1-6] | fgrep "i h_hs" | egrep -v -f /usr/tmp/skip.ia > /usr/tmp/ia.noms
	findbase < /usr/tmp/ia.noms > /usr/tmp/ia.base
	fgrep 0 /usr/tmp/ia.base | awk '{print $$1}' | rev | sort -d | rev > /usr/tmp/ia.failed

ls.morph: ${LSDIR}/lemmata
	fgrep ":le:" stemsrc/vbs.latin.bas | cut -d: -f3 | sort > stemsrc/lemmata
	sed -e 's/\-//' ${LSDIR}/lemmata |  splitlat | fgrep -v -f vtags | fgrep -v "<pos>P. a.</pos>" |  newlems -L | fgrep -v ": " | newlats > /usr/tmp/ls.morph
	sed -e 's/\-//' ${LSDIR}/lemmata |  combitype | splitlat | conj1 | fgrep -f vtags | latvb > ls.vbs
	egrep -e "^:" ls.vbs > stemsrc/vbs.latin
	fgrep -v "0:" /usr/tmp/ls.morph > stemsrc/ls.nom
	cp /usr/tmp/ls.morph .