SUBDIRS = . tests

opdir        = $(datadir)/vyatta-op/templates

share_perl5dir	 = $(datarootdir)/perl5/Vyatta/VPN

libudevdir	 = /lib/udev
etcudevdir	 = /etc/udev
initddir	 = /etc/init.d
logrotatedir     = /etc/logrotate.d
strongswandir    = /etc/strongswan.d
tmplscriptdir = $(datadir)/tmplscripts
yangdir = /usr/share/configd/yang
dhcphookdir = /etc/dhcp/dhclient-exit-hooks.d

sbin_SCRIPTS =
bin_SCRIPTS =

sbin_SCRIPTS += scripts/vfp-config.pl
sbin_SCRIPTS += scripts/vpn-config.pl
sbin_SCRIPTS += scripts/vyatta-gen-x509-keypair.sh
sbin_SCRIPTS += scripts/vyatta-vti-config.pl
sbin_SCRIPTS += scripts/vyatta-s2s-config
sbin_SCRIPTS += scripts/vyatta-ipsec-rekey
sbin_SCRIPTS += scripts/vyatta-security-vpn-secrets
sbin_SCRIPTS += scripts/vyatta-ike-sa-daemon
sbin_SCRIPTS += scripts/vpn-config-vici
sbin_SCRIPTS += scripts/vyatta-op-vpn-ipsec-vici
sbin_SCRIPTS += scripts/vpn-config-global-ike

bin_SCRIPTS += scripts/convert_key_to_rsasig.pl
bin_SCRIPTS += scripts/generate_new_rsasig.pl
bin_SCRIPTS += scripts/gen_local_rsa_key.pl
bin_SCRIPTS += scripts/vyatta-ipsec-dhcp.pl
bin_SCRIPTS += scripts/vyatta-op-vpn.pl
bin_SCRIPTS += scripts/vyatta-show-vpn-debug.sh
bin_SCRIPTS += scripts/vyatta-show-vpn.pl
bin_SCRIPTS += scripts/vyatta-vpn-op.pl
bin_SCRIPTS += scripts/vyatta-pki

share_perl5_DATA = lib/Vyatta/VPN/OPMode.pm
share_perl5_DATA += lib/Vyatta/VPN/Util.pm
share_perl5_DATA += lib/Vyatta/VPN/Config.pm
share_perl5_DATA += lib/Vyatta/VPN/VTIIntf.pm
share_perl5_DATA += lib/Vyatta/VPN/Constants.pm
share_perl5_DATA += lib/Vyatta/VPN/Charon.pm

logrotate_DATA = etc/logrotate.d/vyatta-ipsec-logs

cpiop = find  . ! -regex '\(.*~\|.*\.bak\|.*\.swp\|.*\#.*\#\)' -print0 | \
  cpio -0pd

install-exec-hook:
	mkdir -p $(DESTDIR)/etc/ppp/ip-up.d
	mkdir -p $(DESTDIR)/etc/ppp/ip-down.d
	mkdir -p $(DESTDIR)$(dhcphookdir)
	mkdir -p $(DESTDIR)/usr/lib/ipsec/
	mkdir -p $(DESTDIR)/var/lib/vyatta-security-vpn/vfp/
	mkdir -p $(DESTDIR)/var/lib/vyatta-security-vpn/vti/
	mkdir -p $(DESTDIR)/etc/systemd/system/strongswan.service.d/
	cp scripts/ipsecd-dhclient-hook $(DESTDIR)$(dhcphookdir)/ipsecd
	install -m755 -t $(DESTDIR)/usr/lib/ipsec/ scripts/vti-up-down.sh \
		scripts/vyatta-dataplane-s2s-updown \
		scripts/vyatta-updown-helper.sh \
		scripts/vyatta-ext-fetcher
	mkdir -p $(DESTDIR)$(tmplscriptdir)
	cd tmplscripts && $(cpiop) $(DESTDIR)$(tmplscriptdir)
	mkdir -p $(DESTDIR)$(yangdir)
	cd yang && $(cpiop) $(DESTDIR)$(yangdir)
	mkdir -p $(DESTDIR)$(sysconfdir)
	cp scripts/key-pair.template $(DESTDIR)$(sysconfdir)
	mkdir -p $(DESTDIR)$(opdir)
	cd templates && $(cpiop) $(DESTDIR)$(opdir)

coverage:
	cover -t -make 'prove -r tests;'

check:
	perlcritic -5 .
	pylint3 -d R,too-many-lines,invalid-name,missing-docstring \
		scripts/vyatta-ike-sa-daemon
