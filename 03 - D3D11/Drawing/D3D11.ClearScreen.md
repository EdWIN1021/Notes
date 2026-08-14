```CPP
float colorAsFloats [ 4 ];
constexpr float RGB_SCALE = 255.f;

colorAsFloats [ 0 ] = clearColor.r / RGB_SCALE;
colorAsFloats [ 1 ] = clearColor.g / RGB_SCALE;
colorAsFloats [ 2 ] = clearColor.b / RGB_SCALE;
colorAsFloats [ 3 ] = clearColor.a / RGB_SCALE;

m_deviceContext->ClearRenderTargetView( m_mainRenderTargetView, colorAsFloats );
```