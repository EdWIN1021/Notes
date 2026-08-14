```cpp
D3D11_BUFFER_DESC bufferDesc = {};

bufferDesc.ByteWidth      = sizeof( Vertex ) * 65535;
bufferDesc.Usage          = D3D11_USAGE_DYNAMIC;
bufferDesc.BindFlags      = D3D11_BIND_VERTEX_BUFFER;
bufferDesc.CPUAccessFlags = D3D11_CPU_ACCESS_WRITE;

m_device->CreateBuffer( &bufferDesc, nullptr, &m_immediateVBO );
```