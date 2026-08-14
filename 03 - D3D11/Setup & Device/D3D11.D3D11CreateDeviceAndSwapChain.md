
```cpp
DXGI_SWAP_CHAIN_DESC sd = {};

// Set the number of buffers (1 front buffer, 1 back buffer)
sd.BufferCount = 2;

// Set width and height (0 means auto-match the window size)
sd.BufferDesc.Width = 0;
sd.BufferDesc.Height = 0;

// Set the color format to 8-bit RGBA normalized
sd.BufferDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;

// Mark this buffer as a destination for rendering output
sd.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;

// Provide the handle to the window where we will draw
sd.OutputWindow = reinterpret_cast< HWND >( g_engine->m_window->m_windowHandle );

// Set multi-sampling count to 1 (No Anti-aliasing for now)
sd.SampleDesc.Count = 1;

// Start in windowed mode (not full screen)
sd.Windowed = TRUE;

// Use the modern "Flip" model for buffer swapping (Discard old frame)
sd.SwapEffect = DXGI_SWAP_EFFECT_FLIP_DISCARD;
```

---


```CPP
// Logic to enable the Debug Layer if we are in Debug mode

	UINT createDeviceFlags = 0;
#if defined( DEBUG_MODE )
	createDeviceFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif
```

---

```CPP
D3D_FEATURE_LEVEL featureLevel;
```

---

```cpp
HRESULT hr = D3D11CreateDeviceAndSwapChain( 
	nullptr,                            // Use default graphics adapter
	D3D_DRIVER_TYPE_HARDWARE,           // Use hardware acceleration
	nullptr,                            // No software rasterizer
	createDeviceFlags,                  // Debug/Normal flags
	nullptr, 0,                         // Use default feature levels
	D3D11_SDK_VERSION,                  // Standard SDK version
	&sd,                                // The description we just filled
	&m_swapChain,                       // Output: Swap Chain
	&m_device,                          // Output: Device
	&featureLevel,                      // Output: Actual feature level
	&m_deviceContext                    // Output: Context
);
```