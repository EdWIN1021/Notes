
```CPP
// Create a Render Target View (RTV) to use that texture as a canvas
m_device->CreateRenderTargetView( backBuffer, nullptr, &m_mainRenderTargetView );
	
// Release the temporary reference to the back buffer
backBuffer->Release();
```