# Download and build libtfm from source with the options we want.

# Commit against which we've been testing.  Perhaps we should be using
# a git subrepository instead of this hack?

URL	:= https://github.com/libtom/tomsfastmath.git
COMMIT	:= e0fe602802e376a971a84ba300ad0aab17165600

# Maximum size of a bignum.  See tfm.pdf section 1.3.6 ("Precision
# configuration") for details on how FP_MAX_SIZE works.

BITS	:= 8192


REPO	:= $(notdir $(basename ${URL}))
HDR	:= ${REPO}/src/headers/tfm.h
LIB	:= ${REPO}/libtfm.a

FLAGS	:= CFLAGS='-fPIC -Wall -W -Wshadow -Isrc/headers -g3 -DFP_MAX_SIZE="(${BITS}*2+(8*DIGIT_BIT))"'

TARGETS	:= $(notdir ${HDR} ${LIB})

SHA256SUM := $(firstword $(wildcard /usr/local/bin/sha256sum /usr/local/bin/gsha256sum /usr/bin/sha256sum))

all: ${TARGETS}

clean:
	rm -f ${TARGETS} $(notdir ${HDR}.tmp)
	cd ${REPO}; git clean -dxf

distclean: clean
	rm -rf ${REPO} TAGS

${HDR}:
	git clone --quiet --no-checkout ${URL}
	cd ${REPO}; git checkout --quiet ${COMMIT}

${LIB}: ${HDR}
ifeq "" "${SHA256SUM}"
	@echo "Couldn't find sha256sum, not verifying distribution checksums"
else
	${SHA256SUM} --check Checksums
endif
	cd ${REPO}; git clean -dxf
	cd ${REPO}; ${MAKE} ${FLAGS}

$(notdir ${HDR}): ${HDR}
	echo  >$@.tmp '/* Configure size of largest bignum we want to handle -- see notes in tfm.pdf */'
	echo >>$@.tmp '#define   FP_MAX_SIZE   (${BITS}*2+(8*DIGIT_BIT))'
	echo >>$@.tmp ''
	cat  >>$@.tmp $^
	mv -f $@.tmp $@

$(notdir ${LIB}): ${LIB}
	ln -f $^ $@

tags: TAGS

TAGS: ${HDR}
	find ${REPO} -type f -name '*.[ch]' -print | etags -

ifneq "" "${SHA256SUM}"
regenerate-checksums: ${HDR}
	cd ${REPO}; git clean -dxf
	find ${REPO} -name .git -prune -o -type f -print | sort | xargs ${SHA256SUM} >Checksums
endif
