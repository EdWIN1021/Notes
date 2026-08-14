---
id: Initializing Direct3D
aliases: []
tags: []
---

# Create the Device and Context
- `ID3D11Device` interface is used to check feature support, and allocate resources.
- `ID3D11DeviceContext` interface is used to set render states, bind resources to the graphics pipeline and issue rendering commands.

```cpp
HRESULT D3D11CreateDevice(
  IDXGIAdapter *pAdapter,
  D3D_DRIVER_TYPE DriverType,
  HMODULE Software,
  UINT Flags,
  CONST D3D_FEATURE_LEVEL *pFeatureLevels,
  UINT FeatureLevels,
  UINT SDKVersion,
  ID3D11Device **ppDevice,
	D3D_FEATURE_LEVEL *pFeatureLevel,
	ID3D11DeviceContext **ppImmediateContext
);
```

- `pAdapter` specifies the DXGI adapter (graphics card) that the Direct3D device and swap chain will use for rendering.
- `DeviceType` D3D_DRIVER_TYPE_HARDWARE
- `Software` null
- `Flags`
	- D3D11_CREATE_DEVICE_DEBUG: Direct3D will send debug messages to the VC++ output window
	- D3D11_CREATE_DEVICE_SINGLETHREADED: If this flag is enabled, then the `ID3D11Device::CreateDeferredContext` method will fail
- `pFeatureLevels`D3D_FEATURE_LEVEL_11_0 (Full Direct3D 11 support)
