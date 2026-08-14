```CPP
D3D11_VIEWPORT viewport = {};

viewport.TopLeftX = 0.f;                           // Start from the top-left X
viewport.TopLeftY = 0.f;                           // Start from the top-left Y
viewport.Width    = 1600.f;                        // Drawing width
viewport.Height   = 800.f;                         // Drawing height
viewport.MinDepth = 0.f;                           // Minimum Z depth
viewport.MaxDepth = 1.f;                           // Maximum Z depth

// Finalize the viewport on the Context
m_deviceContext->RSSetViewports( 1, &viewport );
```