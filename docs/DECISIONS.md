# Decisions

Choices made while porting to the iPhone Duo that are not covered by the
existing docs.

## Bundle identifier `us.devusb.ra2`

`build-ios.sh` defaults to `com.ammaar.ra2web`, which belongs to another
developer team and cannot be provisioned by this fork's team. Builds set
`RA2_BUNDLE_ID=us.devusb.ra2`.

## Device builds target the device by id

With a free (personal) team, `xcodebuild -destination 'generic/platform=iOS'`
fails with "Your team has no devices from which to generate a provisioning
profile". Building with `-destination 'id=<device UDID>'` registers the device
and creates the profile. Device builds run `xcodebuild` directly with the
device id instead of `build-ios.sh --device`.

## AI liveness gate skipped for unmodified builds

`build-ios.sh --device` requires `RA2_LIVENESS_OK=1`. The first Duo build was
of unmodified upstream code, so the gate was set without running the probe.
Run the probe before any build that changes simulation or AI code.

## Options panel scrolls instead of shrinking

The general options panel (748px of content in a 568px container on the Duo's
inner display, 870px in 568px on desktop) now scrolls. Tightening spacing or
hiding rows on short layouts was not chosen because the content exceeds the
container by ~30% and would still overflow on shorter screens.

## Reverse touch scroll is an option, off by default

Two-finger drag pans with the map content following the fingers. Reversed
panning, where the camera moves in the drag direction, is available through
the Reverse Touch Scroll option in General options on touch devices. The
default is unchanged.
