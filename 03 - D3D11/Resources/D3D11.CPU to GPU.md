```CPP
D3D11_MAPPED_SUBRESOURCE mappedResource = {};

HRESULT hr = m_deviceContext->Map( vbo, 0, D3D11_MAP_WRITE_DISCARD, 0, &mappedResource );

if ( SUCCEEDED( hr ) )
{
	size_t totalBytes = sizeof( Vertex ) * numVertexes;
	memcpy( mappedResource.pData, vertexes, totalBytes );
	m_deviceContext->Unmap( vbo, 0 );
}
```