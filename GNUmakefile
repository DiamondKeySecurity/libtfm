# Download and build libtfm from source with the options we want.
#
# Perhaps we should be using a git subrepository instead of this hack?
# Work that out later.

# See tfm.pdf section 1.3.6 ("Precision configuration") for details on
# how FP_MAX_SIZE works.

BITS	:= 8192

URL	:= https://github.com/libtom/tomsfastmath.git

REPO	:= $(notdir $(basename ${URL}))
HDR	:= ${REPO}/src/headers/tfm.h
LIB	:= ${REPO}/libtfm.a

FLAGS	:= CFLAGS='-fPIC -Wall -W -Wshadow -Isrc/headers -g3 -DFP_MAX_SIZE="(${BITS}+(8*DIGIT_BIT))"'

TARGETS	:= $(notdir ${HDR} ${LIB})

all: ${TARGETS}

clean:
	rm -f ${TARGETS} $(notdir ${HDR}.tmp)
	cd ${REPO}; git clean -dxf

distclean: clean
	rm -rf ${REPO} TAGS

${HDR}:
	git clone -q ${URL}

${LIB}: ${HDR}
#	sha256sum --check Checksums
	cd ${REPO}; git clean -dxf
	cd ${REPO}; ${MAKE} ${FLAGS}

$(notdir ${HDR}): ${HDR}
	echo  >$@.tmp '/* Configure size of largest bignum we want to handle -- see notes in tfm.pdf */'
	echo >>$@.tmp '#define   FP_MAX_SIZE   (${BITS}+(8*DIGIT_BIT))'
	echo >>$@.tmp ''
	cat  >>$@.tmp $^
	mv -f $@.tmp $@

$(notdir ${LIB}): ${LIB}
	ln -f $^ $@

tags: TAGS

TAGS: ${HDR}
	find ${REPO} -type f -name '*.[ch]' -print | etags -
