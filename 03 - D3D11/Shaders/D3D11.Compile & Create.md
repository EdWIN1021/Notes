```CPP
const char* shaderSource = R"(
	struct vs_input_t {
		float3 position : POSITION;
		float4 color    : COLOR;
	};
	struct v2p_t {
		float4 position : SV_Position;
		float4 color    : COLOR;
	};
	v2p_t VertexMain(vs_input_t input) {
		v2p_t v2p;
		v2p.position = float4(input.position, 1.0f);
		v2p.color = input.color;
		return v2p;
	}
	float4 PixelMain(v2p_t input) : SV_Target {
		return input.color;
	}
)";
```

---

```CPP
ID3DBlob* vsByteCode = nullptr;
ID3DBlob* psByteCode = nullptr;
ID3DBlob* errors = nullptr;

D3DCompile( shaderSource, strlen( shaderSource ), nullptr, nullptr, nullptr, "VertexMain", "vs_5_0", 0, 0, &vsByteCode, &errors );
m_device->CreateVertexShader( vsByteCode->GetBufferPointer(), vsByteCode->GetBufferSize(), nullptr, &m_vertexShader );

D3DCompile( shaderSource, strlen( shaderSource ), nullptr, nullptr, nullptr, "PixelMain", "ps_5_0", 0, 0, &psByteCode, &errors );
m_device->CreatePixelShader( psByteCode->GetBufferPointer(), psByteCode->GetBufferSize(), nullptr, &m_pixelShader );
```