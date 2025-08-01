.. _nrf_802154_changelog:

Changelog
#########

.. contents::
   :local:
   :depth: 2

All notable changes to this project are documented in this file.
See also :ref:`nrf_802154_limitations` for permanent limitations.

Main branch - nRF 802.15.4 Radio Driver
***************************************

Notable changes
===============

* The configuration macro :c:macro:`NRF_802154_CCA_ED_THRESHOLD_DEFAULT` is deprecated.
  It is superseded by the :c:macro:`NRF_802154_CCA_ED_THRESHOLD_DBM_DEFAULT` macro, which specifies the absolute value of the CCA ED threshold in dBm. (KRKNWK-19974)
* The Energy Detection (ED) sample, Received Signal Strength Indicator (RSSI), and Clear Channel Assessment (CCA) threshold now account for Low-Noise Amplifier (LNA) gain.
  These values now represent the power at the antenna connector, rather than at the radio. (KRKNWK-19974)
* The reliability of timestamping on nRF52 and nRF53 Series SoCs was improved.
  The driver no longer reports :c:macro:`NRF_802154_NO_TIMESTAMP` for received
  frames and ACKs in rare cases. (KRKNWK-20539)

Added
=====

* Added the :c:func:`nrf_802154_alternate_short_address_set` function, which allows for setting the secondary short address that will be accepted by the frame filter. (KRKNWK-20051)
* Added the :c:func:`nrf_802154_clock_hfclk_latency_set` function, which sets the HFXO startup latency.
  Currently, it is supported only on nRF54L Series SoCs.
  Use this API during clock platform initialization.
  If the latency is not set, the driver assumes a default worst-case startup latency of 1650 us.
* When the FEM power amplifier is configured, the Errata 56 for nRF54L15 is automatically applied. (KRKNWK-20409)

Removed
=======

* Removed the deprecated :c:func:`nrf_802154_state_get` function. (KRKNWK-17467)
* Removed the deprecated :c:func:`nrf_802154_tx_started` callout function. (KRKNWK-17467)
* Removed the deprecated :c:func:`nrf_802154_buffer_free_immediately_raw` function. (KRKNWK-17467)

nRF Connect SDK v3.0.0 - nRF 802.15.4 Radio Driver
**************************************************

Added
=====

* For the nRF54L Series, added the :c:macro:`NRF_802154_CCAIDLE_TO_TXEN_EXTRA_TIME_US` configuration macro that allows to extend the time between the CCAIDLE event and the trigger of the TXEN task. (KRKNWK-19819)
* Added the :c:func:`nrf_802154_delayed_trx_receive_scheduled_cancel` function, enabling cancellation of a scheduled reception window without impacting reception windows already in progress. (KRKNWK-19969)

Bug fixes
=========

* Fixed the constant that describes the time between CCAIDLE and READY events.
  This constant is used to calculate the transmission time when using CCA for the nRF54L Series. (KRKNWK-19819)
* Fixed an issue where the timestamp of a received frame indicated a later time
  than the actual end of the frame in the air. (KRKNWK-18121)
* Fixed an issue where an assertion could occur on nRF54L Series SoCs when RADIO shorts were triggered immediately after the DISABLE task, by ensuring shorts are cleared first. (KRKNWK-19574)

Removed
=======

* Removed deprecated non-raw API that could be enabled by setting ``NRF_802154_USE_RAW_API=0``.
  Only raw API is kept.

nRF Connect SDK v2.9.0 - nRF 802.15.4 Radio Driver
**************************************************

Added
=====

* Added support for the nRF54L05 and nRF54L10 SoCs.

nRF Connect SDK v2.8.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* If a time slot ends while waiting for or receiving an ACK frame, the transmission terminates with the :c:macro:`NRF_802154_TX_ERROR_NO_ACK` error code.
  This behavior allows the higher layer to distinguish between a frame that was not transmitted and a frame that was transmitted but did not receive an ACK frame. (KRKNWK-19126)
* When the nRF 802.15.4 Radio Driver prepares for a reception but no free buffer is left, the :c:func:`nrf_802154_receive_failed` callout is generated with a new error code :c:macro:`NRF_802154_RX_ERROR_NO_BUFFER`. (KRKNWK-19304)
* The default assignment of the DPPI channels on the nRF54L Series is changed so that the channels 14 and 15 are left unused for other purposes. (KRKNWK-19349)
* The binaries of the nRF 802.15.4 SL library for the nRF54L15 SoC are provided also for the non-secure operation. (KRKNWK-19338)
* The internal implementation of *notification* module is selected by the :c:macro:`NRF_802154_NOTIFICATION_IMPL` configuration macro.
  The internal implementation of *request* module is selected by the :c:macro:`NRF_802154_REQUEST_IMPL` configuration macro.
* Introduced limited support for receiving and transmitting multipurpose frames. (KRKNWK-19492)
* The driver no longer inserts HT2 termination into authenticated Enh-Acks when Information Element injection is performed.
  When a frame has Header IEs, no Payload IEs, and no MAC payload, HT2 will not be inserted regardless of the frame security field. (KRKNWK-16856)

Added
=====

* Added the :c:macro:`NRF_802154_EGU_USED_CHANNELS_MASK` to inform about the fixed EGU channels used by the driver. (KRKNWK-19408)
* Added the functions :c:func:`nrf_802154_cst_writer_period_set` and :c:func:`nrf_802154_cst_writer_anchor_time_set`. (KRKNWK-19492)

nRF Connect SDK v2.7.0 - nRF 802.15.4 Radio Driver
**************************************************

Bug fixes
=========

* Fixed an issue causing the driver to report a very inaccurate timestamp if a delayed operation starts shortly after sleep request. (KRKNWK-18589)
* Fixed an issue causing the build for the nRF54L15 SoC with :kconfig:option:`CONFIG_FPU` set to ``y`` to fail. (KRKNWK-19373)

nRF Connect SDK v2.6.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* Added the :c:func:`nrf_802154_rx_on_when_idle_set` function which allows to choose between the receive and sleep states during radio idle periods. (KRKNWK-17962)
* Added a safeguard in the :c:func:`nrf_802154_delayed_trx_receive` to disallow scheduling of two delayed reception windows with the same value of ``id`` parameter. (KRKNWK-18263)
* The encryption module for the nRF52 and nRF53 series' SoCs based on the ECB peripheral uses the :c:func:`nrf_802154_sl_ecb_block_encrypt` function. (KRKNWK-18576)
  The :c:func:`nrf_802154_sl_ecb_block_encrypt` provided by the closed-source SL uses :ref:`mpsl` to share the ECB peripheral in the multiprotocol scenario.

Added
=====

* Added the :c:func:`nrf_802154_security_key_remove_all` function that allows you to remove all the stored security keys. (KRKNWK-18108)
* Added :c:macro:`NRF_802154_MAX_PENDING_NOTIFICATIONS` that sets the maximum number of simultaneously pending notifications the driver can issue. (KRKNWK-18110)
* Added an assert abstraction layer to allow for the customization of the detection and handling of abnormal conditions. (KRKNWK-18116)
* Added the possibility to insert the transmission channel value to the transmitted frame metadata. (KRKNWK-17965)
* Added the :c:func:`nrf_802154_ack_data_remove_all` function that allows you to remove all the stored Ack data of a given type. (KRKNWK-18334)

Removed
=======

* Removed the :file:`nrf_802154_debug_assert.c` file. (KRKNWK-18116)
* Removed the deprecated API for the :c:func:`nrf_802154_energy_detected` function. (KRKNWK-17573)
  Removed the code selected by the ``NRF_802154_ENERGY_DETECTED_VERSION=0`` API migration macro.
  Removed the ``NRF_802154_ENERGY_DETECTED_VERSION`` API migration macro itself.

Bug fixes
=========

* Fixed an issue causing the radio in the nRF54H20 PDK EngA to hang in an intermediate state while the radio is being disabled. (KRKNWK-18223)

nRF Connect SDK v2.5.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* The callout function :c:func:`nrf_802154_energy_detected` now takes a parameter of type :c:struct:`nrf_802154_energy_detected_t` and provides the ED result in dBm.
  This change in public API can be enabled by setting the ``NRF_802154_ENERGY_DETECTED_VERSION`` to 1. (KRKNWK-17141)
* Include files with API common for both driver and serialization interfaces are now available in the ``common`` directory.
  This change only affects users who are not using the CMake build system. (KRKNWK-17186)

Added
=====

* Added :c:func:`nrf_802154_timestamp_end_to_phr_convert` and :c:func:`nrf_802154_timestamp_phr_to_shr_convert` that can be used to convert the timestamps used by the driver to the timestamp of the first symbol of frame's PHR. (KRKNWK-17153)
* Added support for :c:func:`nrf_802154_pan_coord_get` through serialization (disabled by default via ``NRF_802154_PAN_COORD_GET_ENABLED``). (KRKNWK-10908)
* Added the possibility to perform multiple CCA attempts before a delayed transmission in case the first CCA attempt detects busy channel. (KRKNWK-17304)

Bug fixes
=========
* Fixed an issue causing CSMA/CA procedure to not be terminated correctly in certain Wi-Fi Coexistence scenarios. (KRKNWK-17422)
* Fixed an issue causing data corruption when transmitting frames and ACKs containing IE elements. (KRKNWK-17627)
* Fixed an issue causing an incorrect driver state after transmission setup failure resulting in failing subsequent calls to the 802.15.4 driver. (KRKNWK-17628)

Other changes
=============

* Changed the value of ``ED_RSSISCALE`` to ``4`` for the nRF5340 and nRF52833. (KRKNWK-16902)
* Deprecated :c:func:`nrf_802154_first_symbol_timestamp_get` and :c:func:`nrf_802154_mhr_timestamp_get` functions.
* Improved the modulation filtering when using an external power amplifier on the nRF5340, fixing potential certification issues. (KRKNWK-16949)
* Removed deprecated functions :c:func:`nrf_802154_wifi_coex_enable` and :c:func:`nrf_802154_wifi_coex_disable` and accompanying configuration option ``NRF_802154_COEX_INITIALLY_ENABLED``. (KRKNWK-14574)
* The :c:macro:`NRF_802154_IFS_ENABLED` is disabled by default. IFS feature is marked as experimental. (KRKNWK-17198).

nRF Connect SDK v2.4.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* Improved frame filtering routine which reduces the likelihood of encountering ``NRF_802154_RX_ERROR_RUNTIME`` error during heavier loads. (KRKNWK-15525)
* Delayed transmissions and receptions are triggered by a hardware timer what makes them more immune to software latencies. (KRKNWK-8615)

Added
=====

* Added :c:func:`nrf_802154_security_global_frame_counter_set_if_larger`. (KRKNWK-16133)

Bug fixes
=========
* Fixed an issue causing the notification about transmission failure to be generated twice what led to a crash on the nRF5340 network core. (KRKNWK-16825)
* Fixed an issue with the receive filter, which led to the receiver not being able to receive a frame shorter than 5 bytes in promiscuous mode. (KRKNWK-16977)

Other changes
=============

* Removed the ``NRF_802154_DISABLE_BCC_MATCHING`` config option. Setting this option to ``NRF_802154_DISABLE_BCC_MATCHING=1`` had been not functional for multiple releases. (KRKNWK-15525)
* Removed the ``NRF_802154_TX_STARTED_NOTIFY_ENABLED`` config option. (KRKNWK-16364)
* The total times measurement feature is turned off. (KRKNWK-16189)
* Removed the ``NRF_802154_TOTAL_TIMES_MEASUREMENT_ENABLED`` config option and support for the total times measurement feature. (KRKNWK-16374)
* CSL Phase is calculated assuming that provided CSL anchor time points to a time where the first bit of MAC header of the frame received from a peer happens. (KRKNWK-16647)


nRF Connect SDK v2.3.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* Added the possibility to disable the continuous and modulated carrier functions by setting the ``NRF_802154_CARRIER_FUNCTIONS_ENABLED`` define to ``0``.

nRF Connect SDK v2.2.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* The CSL phase calculation method now depends on the anchor time instead of the nearest scheduled reception window. (KRKNWK-15150)

Added
=====

* Added :c:func:`nrf_802154_csl_writer_anchor_time_set`. (KRKNWK-15150)

Bug fixes
=========

* Implemented a workaround for the YOPAN-158 errata for nRF5340. (KRKNWK-15473)

nRF Connect SDK v2.1.0 - nRF 802.15.4 Radio Driver
**************************************************

Bug fixes
=========

* Fixed an issue where the channel for the delayed transmission on the nRF5340 SoC when passing NULL metadata would be set to 11.
  This was inconsistent with the behavior on nRF52 Series' SoCs and the channel now defaults to the value in the Personal Area Network Information Base (PIB). (KRKNWK-13539)
* Fixed an issue causing the calculated CSL phase to be too small. (KRKNWK-13782)
* Fixed an issue causing the nRF5340 SoC to prematurely run out of buffers for received frames on the application core. (KRKNWK-12493)
* Fixed an issue causing the nRF5340 SoC to transmit with minimum power when the requested transmit power was greater than 0 dBm. (KRKNWK-14487)

nRF Connect SDK v2.0.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* Reworked the implementation of the internal timer to support 64-bit timestamps. (KRKNWK-8612)
* The transmit power is now expressed as antenna output power, including any front-end module used.

Added
=====

* The transmit power can be set for each transmission request through the transmit metadata. (KRKNWK-13484)
* The use of runtime gain control of the front-end module is now provided by the MPSL library. (KRKNWK-13713)

Bug fixes
=========

* Fixed a stability issue where switching the GRANT line of the coexistence interface could cause a crash. (KRKNWK-11900)
* Fixed an issue where the setting ``NRF_802154_DELAYED_TRX_ENABLED=0`` would make the build fail.
* Fixed an issue where the CSMA-CA procedure was not aborted by pending operations with higher priority.
* Fixed an issue where a notification about an HFCLK change could be delayed by a high priority ISR and could cause a crash. (KRKNWK-11466)
* Fixed an issue where canceling a delayed time slot (for CSMA-CA, delayed transmission, and delayed reception operations) after the preconditions were requested could cause a crash. (KRKNWK-13175)
* Fixed an issue where a coexistence request would not be released at the end of the time slot while operating in multiprotocol mode.
* Fixed an issue where the reported ED values with temperature correction were imprecise. (KRKNWK-13599)
* Disabled the build of CSMA-CA when using the open-source service layer.

Other changes
=============

* Removed the files :file:`nrf_802154_ack_timeout.c` and :file:`nrf_802154_priority_drop_swi.c`.

nRF Connect SDK v1.9.0 - nRF 802.15.4 Radio Driver
**************************************************

Added
=====

* Delayed transmission and reception feature support for nRF5340. (KRKNWK-12074)
* Backforwarding of transmitted frames to support retransmissions through serialization for nRF5340. (KRKNWK-10114)
* Serialization of API required by Thread 1.2 (KRKNWK-12077) and other API for nRF5340.

Bug fixes
=========

* Fixed an issue where interleaving transmissions of encrypted and unencrypted frames could cause memory corruption. (KRKNWK-12261)
* Fixed an issue where interruption of a reception of encrypted frame could cause memory corruption. (KRKNWK-12622)
* Fixed an issue where transmission of an encrypted frame could transmit a frame filled partially with zeros instead of proper ciphertext. (KRKNWK-12770)
* Fixed stability issues related to CSMA-CA occurring with enabled experimental coexistence feature from :ref:`mpsl`. (KRKNWK-12701)

nRF Connect SDK v1.8.0 - nRF 802.15.4 Radio Driver
**************************************************

Notable changes
===============

* Incoming frames with Header IEs present but with no payload IEs and with no payload do not need IE Termination Header provided anymore. (KRKNWK-11875)

Bug fixes
=========

* Fixed an issue where the notification queue would be overflowed under stress. (KRKNWK-11606)
* Fixed an issue where ``nrf_802154_transmit_failed`` callout would not always correctly propagate the frame properties. (KRKNWK-11605)

nRF Connect SDK v1.7.0 - nRF 802.15.4 Radio Driver
**************************************************

Added
=====

* Adopted usage of the Zephyr temperature platform for the RSSI correction.
* Support for the coexistence feature from :ref:`mpsl`.
* Support for nRF21540 FEM GPIO interface on nRF53 Series.

Notable changes
===============

* Modified the 802.15.4 Radio Driver Transmit API.
  It now allows specifying whether to encrypt or inject dynamic data into the outgoing frame, or do both.
  The :c:type:`nrf_802154_transmitted_frame_props_t` type is used for this purpose.

Bug fixes
=========

* Fixed an issue where it would not be possible to transmit frames with invalid Auxiliary Security Header if :kconfig:option:`CONFIG_NRF_802154_ENCRYPTION` was set to ``n``. (KRKNWK-11218)
* Fix an issue with the IE Vendor OUI endianness. (KRKNWK-10633)
* Fixed various bugs in the MAC Encryption layer. (KRKNWK-10646)

nRF Connect SDK v1.6.0 - nRF 802.15.4 Radio Driver
**************************************************

Initial common release.

Added
=====

* Added the source code of the 802.15.4 Radio Driver.
* Added the 802.15.4 Service Layer library.
* Added the source code of the 802.15.4 Radio Driver API serialization library.
* Added the possibility to schedule two delayed reception windows.
* Added CSL phase injection.
* Added outgoing frame encryption and frame counter injection.
* Added Thread Link Metrics IEs injection.

Notable Changes
===============

* The release notes of the legacy versions of the Radio Driver are available in the Changelog for 802.15.4 Radio Driver v1.10.0.
* The changelog of the previous versions of the 802.15.4 SL library is now located at the bottom of this page.
* The Radio Driver documentation will now also include the Service Layer documentation.
* Future versions of the Radio Driver and the Service Layer will follow |NCS| version tags.
* The 802.15.4 Radio Driver API has been modified to support more than a single delayed reception window simultaneously.
  The :c:func:`nrf_802154_receive_at`, :c:func:`nrf_802154_receive_at_cancel`, and :c:func:`nrf_802154_receive_failed` functions take an additional parameter that identifies a given reception window unambiguously.

nRF 802.15.4 Service Layer 0.5.0
********************************

* Added the possibility to check the 802.15.4 capabilities.

Added
=====

* Added the possibility to check the 802.15.4 capabilities.
  Built from commit *2966ae8b4b3fcf2b64d8b987703cbf4ecc0dd60b*.

nRF 802.15.4 Service Layer 0.4.0
********************************

* Added multiprotocol support for the nRF53 family.

Added
=====

* Added multiprotocol support for the nRF53 family.
  Built from commit *5d2497b78683687bdd57fcd6854b1bc3c26871be*.

nRF 802.15.4 Service Layer 0.3.0
********************************

* PA/LNA implementation has been moved to MPSL.
  Obsolete implementation and API have been removed.

Removed
=======

* Removed PA/LNA implementation and API.
  Built from commit *e268db75108016ee02965556aa52cf8437f5e071*.

nRF 802.15.4 Service Layer 0.2.0
********************************

Initial release.

Added
=====

* Added the :file:`libnrf_802154_sl.a` library.
  Built from commit *4c5ff68c4eb4ba817774bbd6c711a67dfde7d905*.
