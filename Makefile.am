ACLOCAL_AMFLAGS     = -I m4
DISTCLEANFILES      = *~ DEADJOE semantic.cache *.gdb *.elf core core.* *.d *.sock
DISTCHECK_CONFIGURE_FLAGS = --with-systemd=$$dc_install_base/$(systemd)

confdir             = $(sysconfdir)
conf_DATA           = watchdogd.conf
doc_DATA            = README.md LICENSE
EXTRA_DIST          = README.md LICENSE ChangeLog.md watchdogd.conf
DIST_SUBDIRS        = src man examples
SUBDIRS             = src man
if ENABLE_EXAMPLES
SUBDIRS            += examples
endif

if HAVE_SYSTEMD
systemd_DATA        = watchdogd.service
endif

## Compat hook
install-dev:
	@make -C src install-pkgincludeHEADERS

## Generate .deb package
package:
	@dpkg-buildpackage -uc -us -B

## Generate MD5 checksum file
MD5 = md5sum
md5-dist:
	@for file in $(DIST_ARCHIVES); do	\
		$(MD5) $$file > $$file.md5;	\
	done

## Check if tagged in git
release-hook:
	if [ ! `git tag | grep $(PACKAGE_VERSION)` ]; then				\
		echo;									\
		printf "\e[1m\e[41mCannot find release tag $(PACKAGE_VERSION)\e[0m\n";	\
		printf "\e[1m\e[5mDo release anyway?\e[0m "; read yorn;			\
		if [ "$$yorn" != "y" -a "$$yorn" != "Y" ]; then				\
			printf "OK, aborting release.\n";				\
			exit 1;								\
		fi;									\
		echo;									\
	else										\
		echo;									\
		printf "\e[1m\e[42mFound GIT release tag $(PACKAGE_VERSION)\e[0m\n";	\
		printf "\e[1m\e[44m>>Remember to push tags!\e[0m\n";			\
		echo;									\
	fi

## Target to run when building a release
release: distcheck release-hook md5-dist
	@for file in $(DIST_ARCHIVES); do						\
		printf "$$file    \tDistribution tarball\n";				\
		printf "$$file.md5\t"; cat $$file.md5 | cut -f1 -d' ';			\
		mv $$file* ../;								\
	done

