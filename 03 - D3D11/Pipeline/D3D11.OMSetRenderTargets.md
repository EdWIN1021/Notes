
> Use the Device Context to bind it to the Output Merger stage


```CPP
// Output Merger / Bind the RTV to the Output Merger stage of the GPU
m_deviceContext->OMSetRenderTargets( 
	1,                                         // NumViews
	&m_mainRenderTargetView,                   // RenderTargetViews
	nullptr                                    // Depth Stencil View
);
```