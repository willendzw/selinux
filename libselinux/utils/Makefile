# Installation directories.
PREFIX ?= $(DESTDIR)/usr
LIBDIR ?= $(PREFIX)/lib
USRBINDIR ?= $(PREFIX)/sbin
SBINDIR ?= $(DESTDIR)/sbin
INCLUDEDIR ?= $(PREFIX)/include

MAX_STACK_SIZE=8192
CFLAGS ?= -O -Wall -W -Wundef -Wformat-y2k -Wformat-security -Winit-self -Wmissing-include-dirs \
          -Wunused -Wunknown-pragmas -Wstrict-aliasing -Wshadow -Wpointer-arith \
          -Wbad-function-cast -Wcast-align -Wwrite-strings -Wlogical-op -Waggregate-return \
          -Wstrict-prototypes -Wold-style-definition -Wmissing-prototypes \
          -Wmissing-declarations -Wmissing-noreturn -Wmissing-format-attribute \
          -Wredundant-decls -Wnested-externs -Winline -Winvalid-pch -Wvolatile-register-var \
          -Wdisabled-optimization -Wbuiltin-macro-redefined -Wpacked-bitfield-compat \
          -Wsync-nand -Wattributes -Wcoverage-mismatch -Wmultichar -Wcpp \
          -Wdeprecated-declarations -Wdiv-by-zero -Wdouble-promotion -Wendif-labels -Wextra \
          -Wformat-contains-nul -Wformat-extra-args -Wformat-zero-length -Wformat=2 -Wmultichar \
          -Wnormalized=nfc -Woverflow -Wpointer-to-int-cast -Wpragmas -Wsuggest-attribute=const \
          -Wsuggest-attribute=noreturn -Wsuggest-attribute=pure -Wtrampolines \
          -Wno-missing-field-initializers -Wno-sign-compare -Wjump-misses-init \
          -Wno-format-nonliteral -Wframe-larger-than=$(MAX_STACK_SIZE) -Wp,-D_FORTIFY_SOURCE=2 \
          -fstack-protector-all --param=ssp-buffer-size=4 -fexceptions \
          -fasynchronous-unwind-tables -fdiagnostics-show-option -funit-at-a-time \
          -fipa-pure-const -Wno-suggest-attribute=pure -Wno-suggest-attribute=const \
          -Werror -Wno-aggregate-return -Wno-redundant-decls
override CFLAGS += -I../include -I$(INCLUDEDIR) -D_GNU_SOURCE $(DISABLE_FLAGS) $(PCRE_CFLAGS)
LDLIBS += -L../src -lselinux -L$(LIBDIR)
PCRE_LDFLAGS ?= -lpcre

ifeq ($(ANDROID_HOST),y)
TARGETS=sefcontext_compile
else
TARGETS=$(patsubst %.c,%,$(wildcard *.c))
endif

sefcontext_compile: LDLIBS += $(PCRE_LDFLAGS) ../src/libselinux.a -lsepol

sefcontext_compile: sefcontext_compile.o ../src/regex.o

selinux_restorecon: LDLIBS += -lsepol

all: $(TARGETS)

install: all
	-mkdir -p $(USRBINDIR)
	install -m 755 $(TARGETS) $(USRBINDIR)
	-mkdir -p $(SBINDIR)
clean:
	rm -f $(TARGETS) *.o *~

distclean: clean

indent:
	../../scripts/Lindent $(wildcard *.[ch])

relabel:

