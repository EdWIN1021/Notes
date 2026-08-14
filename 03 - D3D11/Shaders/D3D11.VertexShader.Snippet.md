---
id:
aliases: []
tags: []
---

```hlsl
float4 main( float2 pos : Position ) : SV_Position
{
  return float4( pos.x, pos.y, 0.0f, 1.0f );
}
```

---

```cpp
ID3DBlob* vsByteCode = null;

// vs_5_0 ( Vertex Shader model 5.0 )
D3DCompile( shaderSource, strlen( shaderSource ), nullptr, nullptr, nullptr, "VertexMain", "vs_5_0", 0, 0, &vsByteCode, &errors );
m_device->CreateVertexShader( vsByteCode->GetBufferPointer(), vsByteCode->GetBufferSize(), nullptr, &m_vertexShader );
```



### Concept
[[Concept.D3D11.VertexShader]]
