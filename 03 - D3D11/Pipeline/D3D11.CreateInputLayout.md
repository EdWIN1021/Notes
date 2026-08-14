[[Concept.D3D11.InputLayout]]
[[Snippet.D3D11.VertexShader]]

```CPP
D3D11_INPUT_ELEMENT_DESC layout [] = {
	{ "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
	{ "COLOR",    0, DXGI_FORMAT_R8G8B8A8_UNORM,  0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 }
};

m_device->CreateInputLayout( 
	layout, 
	2, 
	vsByteCode->GetBufferPointer(),
	vsByteCode->GetBufferSize(), 
	&m_inputLayout 
);
```
