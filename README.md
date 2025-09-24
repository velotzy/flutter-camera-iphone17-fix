# Flutter Camera iPhone 17 Fix

This is a forked version of the Flutter camera package that fixes the iPhone 17 compatibility issue.

## Problem

The original Flutter camera package was using `kCVPixelFormatType_32BGRA` pixel format, which is not supported on iPhone 17 devices, causing the error:

```
[AVCaptureVideoDataOutput setVideoSettings:] Unsupported pixel format type - use -availableVideoCVPixelFormatTypes
```

## Solution

This fork implements a dynamic pixel format selection that:

1. **Checks available pixel formats** on the device using `availableVideoCVPixelFormatTypes`
2. **Uses a prioritized list** of preferred formats that are more likely to work on iPhone 17
3. **Falls back gracefully** to the first available format if none of the preferred ones work
4. **Validates formats** before setting them to prevent runtime errors

### Key Changes

- Added `getSupportedVideoFormat` method in `FLTCam.m`
- Updated initialization to use dynamic format selection
- Improved `setVideoFormat:` method with validation
- Better fallback strategy prioritizing iPhone 17 compatible formats

### Preferred Format Priority

1. `kCVPixelFormatType_420YpCbCr8BiPlanarFullRange` (most compatible with iPhone 17)
2. `kCVPixelFormatType_420YpCbCr8BiPlanarVideoRange`
3. `kCVPixelFormatType_32RGBA`
4. `kCVPixelFormatType_422YpCbCr8`
5. `kCVPixelFormatType_422YpCbCr8_yuvs`
6. `kCVPixelFormatType_32BGRA` (fallback for older devices)

## Usage

To use this fix in your Flutter project, update your `pubspec.yaml`:

```yaml
dependencies:
  camera:
    git:
      url: https://github.com/velotzy/flutter-camera-iphone17-fix.git
      path: packages/camera/camera
```

## Testing

This fix has been tested and successfully builds for iOS. The camera functionality should now work properly on iPhone 17 devices without the pixel format error.

## Contributing

This is a temporary fix until the official Flutter camera package is updated. Consider contributing this fix back to the official repository.

## License

Same license as the original Flutter camera package.