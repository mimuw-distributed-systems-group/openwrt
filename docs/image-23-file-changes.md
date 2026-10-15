# image-23 file changes

| Category | Files |
|---|---:|
| [Imported byte for byte](#imported-byte-for-byte) | 76 |
| [Imported, then modified](#imported-then-modified) | 8 |
| [Modified](#modified) | 18 |
| [Custom](#custom) | 7 |
| [Deleted](#deleted) | 61 |
| **Total** | **170** |

---

## Imported byte for byte

### hostapd, from OpenWrt 22.03.7

In `package/network/services/hostapd/`:

- `Config.in`
- `files/dhcp-get-server.sh`
- `files/hostapd-basic.config`
- `files/hostapd-full.config`
- `files/hostapd-mini.config`
- `files/wpa_supplicant-basic.config`
- `files/wpa_supplicant-full.config`
- `files/wpa_supplicant-mini.config`
- `files/wpa_supplicant-p2p.config`
- `files/wpad.init`
- `files/wpad.json`
- `files/wpad_acl.json`
- `files/wps-hotplug.sh`

### hostapd, from OpenWrt 25.12.4

In `package/network/services/hostapd/`. These match the hostap.git commit
`ca266cc24d` (2025-08-26) that 25.12.4 also uses.

`patches/`:

- `010-mesh-Allow-DFS-channels-to-be-selected-if-dfs-is-ena.patch`
- `011-mesh-use-deterministic-channel-on-channel-switch.patch`
- `021-fix-sta-add-after-previous-connection.patch`
- `030-Revert-nl80211-Accept-a-global-nl80211-event-to-a-br.patch`
- `050-Fix-OpenWrt-13156.patch`
- `051-nl80211-add-extra-ies-only-if-allowed-by-driver.patch`
- `052-AP-add-missing-null-pointer-check-in-hostapd_free_ha.patch`
- `060-nl80211-fix-reporting-spurious-frame-events.patch`
- `070-netlink-increase-buffer-size.patch`
- `150-add-NULL-checks-encountered-during-tests-hwsim.patch`
- `160-dpp_pkex-EC-point-mul-w-value-prime.patch`
- `170-hostapd-update-cfs0-and-cfs1-for-160MHz.patch`
- `180-fix_owe_ssid_update.patch`
- `190-hostapd-Fix-hostapd-crash-if-setup-a-iface-with-.patch`
- `191-hostapd-add-support-for-specifying-the-link-id-in-th.patch`
- `192-nl80211-cancel-scans-whenever-the-first-bss-is-remov.patch`
- `200-multicall.patch`
- `201-lto-jobserver-support.patch`
- `211-ctrl-make-WNM_AP-functions-dependant-on-CONFIG_AP.patch`
- `212-Move-definition-of-WLAN_SUPP_RATES_MAX-to-defs.h.patch`
- `213-wpa_supplicant-fix-warnings.patch`
- `220-indicate-features.patch`
- `250-hostapd_cli_ifdef.patch`
- `251-wpa_cli_ifdef.patch`
- `252-disable_ctrl_iface_mib.patch` (replaces image-22's `380-disable_ctrl_iface_mib.patch`)
- `300-noscan.patch`
- `301-mesh-noscan.patch`
- `310-rescan_immediately.patch`
- `320-optional_rfkill.patch`
- `341-mesh-ctrl-iface-channel-switch.patch`
- `350-nl80211_del_beacon_bss.patch`
- `360-acs_retry.patch`
- `370-preserve_radio_mask.patch`
- `381-hostapd_cli_UNKNOWN-COMMAND.patch`
- `400-wps_single_auth_enc_type.patch`
- `410-limit_debug_messages.patch`
- `460-wpa_supplicant-add-new-config-params-to-be-used-with.patch`
- `463-add-mcast_rate-to-11s.patch`
- `464-fix-mesh-obss-check.patch`
- `465-hostapd-config-support-random-BSS-color.patch`
- `470-survey_data_fallback.patch`
- `590-rrm-wnm-statistics.patch`
- `701-reload_config_inline.patch`
- `710-vlan_no_bridge.patch`
- `711-wds_bridge_force.patch`
- `720-iface_max_num_sta.patch`
- `730-ft_iface.patch`
- `740-snoop_iface.patch`
- `760-dynamic_own_ip.patch`
- `761-shared_das_port.patch`
- `762-AP-don-t-ignore-probe-requests-with-invalid-DSSS-par.patch`
- `763-radius-wispr.patch`
- `770-radius_server.patch`

`src/`:

- `src/hostapd/radius.c`
- `src/src/ap/ubus.c`
- `src/src/ap/ubus.h`
- `src/src/ap/ucode.c`
- `src/src/ap/ucode.h`
- `src/src/utils/build_features.h`
- `src/src/utils/ucode.c`
- `src/src/utils/ucode.h`
- `src/wpa_supplicant/ucode.c`
- `src/wpa_supplicant/ucode.h`

---

## Imported, then modified

| File | Source | Changes |
|---|---|---|
| `package/network/services/hostapd/Makefile` | OpenWrt 22.03.7 | 2025-08-26 source; `PKG_RELEASE`/`PKG_VERSION` set by hand (this `package.mk` has no `AUTORELEASE` or source-date versioning); one `PKG_BUILD_DIR` per variant; no `PROVIDES` (this `metadata.pm` then skips the real packages); `Build/Prepare` copies `./src/`; no `libubus` / `-lubox -lubus` (built without ubus) |
| `package/network/services/hostapd/files/hostapd.sh` | OpenWrt 22.03.7 | fallback `json_for_each_item` (missing from this tree's 2015 libubox); `${fils:-0}`; `wpa_supplicant_run` and `hostapd_common_cleanup` from image-22, because 22.03's hand the interface to wpa_supplicant over ubus |
| `package/libs/libcap-ng/Makefile` | OpenWrt 22.03.7 packages feed (`libs/libcap-ng`, 0.8.2) | `PKG_RELEASE:=1`; `+libpthread` (uClibc); no `libcap-ng-bin` package. Needed by OpenVPN 2.7 |
| `package/libs/openssl/patches/100-openwrt_targets.patch` | OpenWrt's openssl package (2018, OpenSSL 1.1.1 era; 25.12.4 has a newer revision as `110-openwrt_targets.patch`) | defines `linux-mipsel-openwrt` for OpenSSL 3's `Configure` |
| `package/libs/openssl/Config.in` | image-22, option text from OpenWrt 25.12.4 | 1.0.2 options (SSLv3, engines) removed; `OPENSSL_WITH_DEPRECATED` added (OpenVPN 2.7 uses deprecated `SSLerr`) |
| `package/network/services/dropbear/files/dropbear.init` | image-22, functions from OpenWrt 22.03.7 | `_dropbearkey`, `hk_verify`, `hk_generate_as_needed` (identical to 22.03.7) replace `keygen`, which needed a DSS key that 2026.94 can't make; DSS option removed; `-D /etc/dropbear` (replaces `100-pubkey_path.patch`); `respawn` |
| `package/network/services/dropbear/patches/130-ssh_ignore_x_args.patch` | OpenWrt 25.12.4 | same change, regenerated against 2026.94 (header and line numbers differ); replaces `130-ssh_ignore_o_and_x_args.patch` |
| `package/network/services/dropbear/patches/140-disable_assert.patch` | OpenWrt 25.12.4 | same change, regenerated against 2026.94 |

---

## Modified

### Build configuration

| File | Change |
|---|---|
| `.config` | see [`.config` settings](#config-settings) below |
| `feeds.conf.default` | `src-link heni ../../heni-software/software/supervisor/openwrt-feed`: the HENI feed from the submodule |
| `tools/automake/files/aclocal` | runs `aclocal.real` from the script's own directory instead of `PATH`; otherwise `PKG_FIXUP:=autoreconf` packages fail to regenerate `aclocal.m4` |
| `target/linux/ramips/dts/OLINUXINO-RT5350F.dts` | identical to heni-image-original-fixed `image-22.4`; differs from this repo's `image-22` tip, which has two later DTS commits (node power/buffer outputs, `jtag_tdo` export, VButton) that were reverted |

### Packages

| File | Change |
|---|---|
| `package/base-files/Makefile` | replaces `172.23.2.1` in `/etc/hosts` with `CONFIG_HENI_SERVER_HOST` |
| `package/base-files/files/etc/hosts` | `172.23.2.1 heni`; config-base ships the same entry, but this file wins in the image |
| `package/base-files/image-config.in` | `HENI_VPN_HOST`, `HENI_SERVER_HOST`, `HENI_FALLBACK_URL` options (dev/prod server addresses) |
| `package/kernel/mac80211/Makefile` | ath10k-firmware source URL changed to a reachable mirror (`github.com/atondwal/ath10k-firmware`) |
| `package/system/usign/Makefile` | source URL `git://git.openwrt.org/...` to `https://github.com/openwrt/usign.git` (anonymous `git://` is gone) |
| `package/libs/libnl-tiny/src/attr.c` | `NLA_S8` minimum length, for the 2025 hostapd |
| `package/libs/libnl-tiny/src/include/netlink/attr.h` | `NLA_S8` and `nla_get_s8()`, for the 2025 hostapd; same as in newer libnl-tiny, written for this version |
| `package/libs/openssl/Makefile` | 1.0.2d to 3.5.7; OpenSSL 3 build (`Configure` target `linux-mipsel-openwrt`, `install_sw`); `+libpthread +libatomic`; unused algorithms and features off to save flash |
| `package/network/config/firewall/files/firewall.config` | wan zone `option input` `REJECT` to `ACCEPT`: the motes are managed (SSH) over wan, and this file wins over config-base's identical setting |
| `package/network/services/dropbear/Makefile` | 2015.67 to 2026.94; `localoptions.h` written to the build dir root (where dropbear reads it); DSS off; ECDSA/ECDH/curve25519 from `CONFIG_DROPBEAR_ECC`; post-quantum key exchange (ML-KEM-768, sntrup761) and ed25519 host keys on; `-std=gnu99` for gcc 4.8 |
| `package/network/services/openvpn/Makefile` | 2.3.6 to 2.7.6, OpenSSL variant only (PolarSSL too old, no-SSL mode removed upstream); `+libcap-ng`; `-std=gnu11`; configure options removed in 2.7 dropped; `PROVIDES:=openvpn-mbedtls` so config-vpn's dependency resolves |
| `package/network/services/openvpn/Config-openssl.in` | options for features removed in OpenVPN 2.7 dropped |
| `package/network/utils/curl/Makefile` | 7.40.0 to 7.88.1 (7.40 can't find SSL support in OpenSSL 3); `--with-ca-bundle=/etc/ssl/certs/ca-certificates.crt` so plain `curl` (fallback-client) verifies HTTPS |
| `package/system/ca-certificates/Makefile` | 20150426 to 20260601; generates the certificates from Mozilla's `certdata.txt` (the source no longer ships them), patching `certdata2pem.py` for the build host's old python3-cryptography; new `ca-bundle` package (`/etc/ssl/certs/ca-certificates.crt`) |

### `.config` settings

| Setting | image-22 | image-23 | Why |
|---|---|---|---|
| `HENI_IMAGE` | `image-20` | `image-23-dev` | image version |
| `HENI_VPN_HOST`, `HENI_SERVER_HOST`, `HENI_FALLBACK_URL` | unset | prod addresses | dev/prod server addresses (`image-config.in`) |
| `DEVEL`, `CCACHE` | off | on | CI build caching |
| `DROPBEAR_ECC` | unset | on | matches what dropbear was actually built with; ECDSA host key |
| `OPENSSL_WITH_DEPRECATED` | unset | on | OpenVPN 2.7 uses deprecated OpenSSL API |
| `OPENVPN_openssl_ENABLE_DEF_AUTH`, `_PF` | on | unset | removed from OpenVPN 2.7 |
| `PACKAGE_dropbear` / `PACKAGE_heni-dropbear` | off / on | on / off | `heni-dropbear` duplicated the same software |
| `PACKAGE_wpad-mini` / `PACKAGE_hostapd`, `PACKAGE_wpa-supplicant` | off / on | on / off | one binary for AP and client, smaller |
| `PACKAGE_heni-openocd` / `PACKAGE_openocd` | unset / on | on / off | HENI's own OpenOCD package |
| `PACKAGE_heni-sftp-server` / `PACKAGE_openssh-sftp-server` | unset / on | on / off | sftp for heni-cli |
| `PACKAGE_ca-bundle` | unset | on | single CA file for curl |
| `PACKAGE_libcap-ng` | unset | on | OpenVPN 2.7 |
| `PACKAGE_libatomic` | off | on | OpenSSL 3 |
| `PACKAGE_lua-bencode`, `PACKAGE_luai2c` | on | off | nothing HENI-owned uses them; dead upstream sources |
| `PACKAGE_libevent2` | on | off | nothing in the image uses it |
| `PACKAGE_rebooter` | on | off | turned off while debugging boot, not turned back on |

---

## Custom

| File | Why |
|---|---|
| `/.gitea/workflows/build.yml` | CI: builds dev and prod images |
| `/.gitmodules`, `/heni-software` | heni repo (`heni/heni-heni`, branch `image-23-backport`) as a submodule; provides the HENI feed |
| `package/libs/libcap-ng/patches/001-drop-utils-uclibc-no-xattr.patch` | doesn't build `utils/` (pscap etc.): not needed, and fails on uClibc (no xattr) |
| `package/libs/libcap-ng/patches/002-uclibc-xattr-syscall-fallback.patch` | `fgetxattr`/`fsetxattr`/`fremovexattr` as raw syscalls and `FIXUP()` for libcs without `<sys/xattr.h>` (uClibc 0.9.33.2) |
| `package/network/services/openvpn/patches/001-uclibc-static_assert-compat.patch` | defines `static_assert` (uClibc's `<assert.h>` predates C11) |
| `package/network/services/hostapd/patches/910-openssl3-ocsp-guard.patch` | no OCSP when OpenSSL is built with `no-ocsp`; only used by hostapd's OpenSSL variants (the image builds `wpad-mini`, which doesn't use OpenSSL) |

---

## Deleted

### openssl: OpenSSL 1.0.x build system patches

Replaced by `Configure` options and `100-openwrt_targets.patch`. In
`package/libs/openssl/patches/`:

- `110-optimize-for-size.patch`
- `130-perl-path.patch`
- `140-makefile-dirs.patch`
- `150-no_engines.patch`
- `160-disable_doc_tests.patch`
- `170-bash_path.patch`
- `180-fix_link_segfault.patch`
- `190-remove_timestamp_check.patch`
- `200-parallel_build.patch`

### dropbear

In `package/network/services/dropbear/patches/`:

| File | Why |
|---|---|
| `100-pubkey_path.patch` | replaced by `-D /etc/dropbear` |
| `110-change_user.patch` | not needed, root logins work (OpenWrt 25.12.4 still carries it) |
| `120-openwrt_options.patch` | edited the old `options.h`; now `localoptions.h` |
| `130-ssh_ignore_o_and_x_args.patch` | replaced by `130-ssh_ignore_x_args.patch` |
| `150-dbconvert_standalone.patch` | fixed upstream |
| `500-set-default-path.patch` | upstream `DEFAULT_ROOT_PATH` includes `/sbin` |

### openvpn

In `package/network/services/openvpn/`:

| File | Why |
|---|---|
| `Config-nossl.in` | no-SSL variant removed (no-SSL mode is gone in 2.7) |
| `Config-polarssl.in` | PolarSSL variant removed |
| `patches/100-polarssl_compat.h` | PolarSSL variant removed |
| `patches/120-polarssl-disable-record-splitting.patch` | PolarSSL variant removed |
| `patches/130-polarssl-disable-runtime-version-check.patch` | PolarSSL variant removed |
| `patches/001-backport_cipher_none_fix.patch` | written for 2.3.6 |
| `patches/110-musl_compat.patch` | written for 2.3.6 |

### curl

Written for 7.40.0. In `package/network/utils/curl/patches/`:

- `010-CVE-2015-3143.patch`
- `011-CVE-2015-3144.patch`
- `012-CVE-2015-3145.patch`
- `013-CVE-2015-3148.patch`
- `014-CVE-2015-3153.patch`
- `015-CVE-2015-3236.patch`
- `016-CVE-2015-3237.patch`
- `100-check_long_long.patch`
- `200-no_docs_tests.patch`
- `300-fix-disable-crypto-auth.patch`
- `310-polarssl-disable-runtime-version-check.patch`

### hostapd: the 2016 package

Replaced by the 22.03.7/25.12.4 files above. In
`package/network/services/hostapd/`:

- `files/netifd.sh`
- `files/wpa_supplicant.sh`
- `patches/100-mesh_mode_fix.patch`
- `patches/120-daemonize_fix.patch`
- `patches/130-no_eapol_fix.patch`
- `patches/140-disable_bridge_packet_workaround.patch`
- `patches/330-nl80211_fix_set_freq.patch`
- `patches/340-reload_freq_change.patch`
- `patches/360-ctrl_iface_reload.patch`
- `patches/370-ap_sta_support.patch`
- `patches/390-wpa_ie_cap_workaround.patch`
- `patches/420-indicate-features.patch`
- `patches/430-hostapd_cli_ifdef.patch`
- `patches/431-wpa_cli_ifdef.patch`
- `patches/432-missing-typedef.patch`
- `patches/450-scan_wait.patch`
- `patches/461-driver_nl80211-use-new-parameters-during-ibss-join.patch`
- `patches/462-wpa_s-support-htmode-param.patch`
- `patches/600-ubus_support.patch`
- `patches/700-160mhz_interop_workaround.patch`
- `patches/901-v2.6-0001-hostapd-Avoid-key-reinstallation-in-FT-handshake.patch`
- `patches/902-v2.6-0002-Prevent-reinstallation-of-an-already-in-use-group-ke.patch`
- `patches/903-v2.6-0003-Extend-protection-of-GTK-IGTK-reinstallation-of-WNM-.patch`
- `patches/904-v2.6-0004-Prevent-installation-of-an-all-zero-TK.patch`
- `patches/905-v2.6-0005-Fix-PTK-rekeying-to-generate-a-new-ANonce.patch`
- `patches/906-v2.6-0006-TDLS-Reject-TPK-TK-reconfiguration.patch`
- `patches/907-v2.6-0007-WNM-Ignore-WNM-Sleep-Mode-Response-without-pending-r.patch`
- `patches/908-v2.6-0008-FT-Do-not-allow-multiple-Reassociation-Response-fram.patch`
