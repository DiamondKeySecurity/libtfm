# Download and build libtfm from source with the options we want.
#
# Perhaps we should be using a git subrepository instead of this hack?
# Work that out later.

URL	:= https://github.com/libtom/tomsfastmath.git

REPO	:= $(notdir $(basename ${URL}))
HDR	:= ${REPO}/src/headers/tfm.h
LIB	:= ${REPO}/libtfm.a

FLAGS	:= CFLAGS='-fPIC -Wall -W -Wshadow -Isrc/headers -g3'

TARGETS	:= $(notdir ${HDR} ${LIB})

all: ${TARGETS}

clean:
	rm -f ${TARGETS}
	cd ${REPO}; git clean -dxf

distclean: clean
	rm -rf ${REPO}

${HDR}:
	git clone -q ${URL}

${LIB}: ${HDR}
#	sha256sum --check Checksums
	cd ${REPO}; git clean -dxf
	cd ${REPO}; ${MAKE} ${FLAGS}

$(notdir ${HDR}): ${HDR}
	ln -f $^ $@

$(notdir ${LIB}): ${LIB}
	ln -f $^ $@
