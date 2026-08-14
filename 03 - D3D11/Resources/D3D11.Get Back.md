> Retrieve the Back Buffer texture from the Swap Chain

```CPP
	ID3D11Texture2D* backBuffer = nullptr;
	m_swapChain->GetBuffer( 0, __uuidof( ID3D11Texture2D ), ( void** ) &backBuffer );
```